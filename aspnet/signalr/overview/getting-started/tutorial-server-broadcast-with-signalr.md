---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'チュートリアル: SignalR 2 によるサーバーがブロードキャスト |Microsoft Docs'
author: tdykstra
description: このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバー ブロードキャストでは、その commun ことを意味しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ff1eeee407ac7628afd587ca8b9102d0191ea356
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367929"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>チュートリアル: SignalR 2 によるサーバーがブロードキャスト
====================
によって[Tom Dykstra](https://github.com/tdykstra)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。 サーバー ブロードキャストでは、クライアントに送信される通信が、サーバーによって開始されたことを意味します。 このシナリオでは、チャット アプリケーションをクライアントに送信される通信が 1 つまたは複数のクライアントによって開始されたなどのピア ツー ピア シナリオよりもさまざまなプログラミングのアプローチが必要です。
> 
> このチュートリアルで作成するアプリケーションでは、株価情報、サーバー ブロードキャストの機能の一般的なシナリオをシミュレートします。
> 
> このトピックでは、Patrick Fletcher によって作成当初されました。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 2 のバージョン
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>このチュートリアルで Visual Studio 2012 の使用
> 
> 
> このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。
> 
> - 更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。
> - インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。
> - Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。 SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。
> - 一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


## <a name="overview"></a>概要

このチュートリアルでは、リアルタイムのアプリケーションを定期的に「プッシュ」する代表的な株価表示器のアプリケーションを作成または接続されているすべてのクライアントをサーバーからの通知をブロードキャストします。 します。 このチュートリアルの最初の部分では、最初からそのアプリケーションの簡略化されたバージョンを作成します。 チュートリアルの残りの部分では、追加の機能を含む NuGet パッケージをインストールし、それらの機能のコードを確認します。

このチュートリアルの最初の部分でビルドするアプリケーションでは、株価データを持つグリッドが表示されます。

![StockTicker 初期バージョン](tutorial-server-broadcast-with-signalr/_static/image1.png)

定期的に、サーバーはランダムに株価を更新し、更新プログラムをすべて接続されているクライアントにプッシュします。 ブラウザー、数字、およびシンボルで、**変更**と**%** 通知をサーバーからの応答で列を動的に変更します。 同じ URL に他のブラウザーを開いた場合、同じデータやデータに同じ変更を同時にすべて表示します。

このチュートリアルには、次のセクションが含まれています。

- [前提条件](#prerequisites)
- [プロジェクトを作成します。](#createproject)
- [サーバー コードを設定します。](#server)
- [クライアント コードを設定します。](#client)
- [アプリケーションをテストします。](#test)
- [ログ記録を有効にします。](#enablelogging)
- [インストールして、完全な StockTicker サンプルを確認してください。](#fullsample)
- [次のステップ](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージは、Visual Studio プロジェクトにサンプルのシミュレートされた株価表示器アプリケーションをインストールします。

> [!NOTE]
> アプリケーションの構築の手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。 このチュートリアルの手順を実行せずに NuGet パッケージをインストールした場合**readme.txt ファイルの指示に従う必要があります**します。 パッケージを実行するには、インストールされているパッケージで ConfigureSignalR メソッドを呼び出す OWIN startup クラスを追加する必要があります。 OWIN startup クラスを追加しない場合、エラーが表示されます。


<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている、Visual Studio 2013 があることを確認します。 Visual Studio を持っていない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)、無料の Visual Studio 2013 Express を取得します。

<a id="createproject"></a>

## <a name="create-the-project"></a>プロジェクトの作成

1. **ファイル** メニューのをクリックして**新しいプロジェクト**します。
2. **新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**します。
3. 選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *SignalR.StockTicker*、 をクリック**OK**します。

    ![[新しいプロジェクト] ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. **新しい ASP.NET** Project ウィンドウのままに**空**選択およびクリックして**プロジェクトの作成**。

    ![新しい ASP プロジェクト ダイアログ ボックス](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>サーバー コードを設定します。

このセクションでは、サーバーで実行されるコードを設定します。

### <a name="create-the-stock-class"></a>在庫クラスを作成します。

まず、格納および転送については、在庫を使用する在庫のモデル クラスを作成します。

1. プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*Stock.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    素材を作成するときに設定する 2 つのプロパティは、(たとえば、Microsoft の MSFT) シンボルと価格は。 その他のプロパティは、価格を設定する方法とタイミングに依存します。 初めての価格を設定する値は、DayOpen に反映を取得します。 価格、変更を設定していて PercentChange プロパティの値を計算以降の時間は、価格と DayOpen の違いに基づきます。

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>StockTicker と StockTickerHub クラスを作成します。

サーバーからクライアントへの対話を処理するために、SignalR Hub API を使用します。 SignalR ハブ クラスから派生した StockTickerHub クラスでは、クライアントからの受信接続とメソッドの呼び出しを処理します。 株価データを維持し、クライアント接続とは無関係に、価格の更新プログラムを定期的にトリガーするタイマー オブジェクトを実行する必要があります。 ハブ インスタンスは一時的なので、ハブ クラスでこれらの関数を配置することはできません。 接続と、クライアントからサーバーへの呼び出しなど、ハブの各操作ではハブ クラスのインスタンスが作成されます。 株価データを保持し、価格を更新、価格の更新プログラムにブロードキャスト メカニズム StockTicker 名前を付けます別のクラスで実行するようにします。

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image5.png)

各 StockTickerHub インスタンスから、シングルトン StockTicker インスタンスへの参照を設定する必要がありますので、サーバー上で実行する StockTicker クラスのインスタンスを 1 つだけします。 株価データには、トリガーの更新プログラム、StockTicker はハブ クラスではないために、クライアントにブロードキャストできる StockTicker クラスにあります。 そのため、StockTicker クラスは、SignalR ハブの接続コンテキスト オブジェクトへの参照を取得しています。 クライアントにブロードキャストする SignalR 接続のコンテキスト オブジェクトを使用できます。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、**追加 |SignalR ハブ クラス (v2)** します。
2. 新しいハブの名前を付けます*StockTickerHub.cs*、 をクリックし、**追加**します。 SignalR の NuGet パッケージをプロジェクトに追加されます。
3. テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)メソッドは、クライアントは、サーバー上で呼び出すことができますを定義するクラスを使用します。 1 つのメソッドを定義する:`GetAllStocks()`します。 クライアントは、最初に、サーバーに接続するときは、その現在の価格の株式のすべての一覧を取得するには、このメソッドを呼び出します。 メソッドは同期的に実行し、返す`IEnumerable<Stock>`メモリからデータを返すためです。 指定したかどうか、メソッドがデータベース検索など、web サービス呼び出しの待機を含むは何かの手順を実行してデータを取得する必要がある`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。 詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)します。

    HubName 属性は、クライアントでの JavaScript コードでのハブの参照方法を指定します。 この属性を使用しない場合、クライアントに既定の名前は、ここで stockTickerHub 可能性のあるクラス名の camel 形式のバージョンです。

    StockTicker クラスを作成するときに、後で表示されます、としては、そのクラスのシングルトン インスタンスがその静的インスタンス プロパティに作成されます。 StockTicker のシングルトン インスタンスまたは切断するには、接続のクライアントの数に関係なくメモリに残りますをそのインスタンスが GetAllStocks メソッドを使用して、現在の株価情報が返されます。
4. プロジェクト フォルダーに新しいクラス ファイルを作成、名前を付けます*StockTicker.cs*、テンプレート コードを次のコードに置き換えます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    複数のスレッドが StockTicker コードの同じインスタンスを実行するため、スレッド セーフにする StockTicker クラスがあります。

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>静的フィールドに、シングルトン インスタンスを格納します。

    静的な初期化\_コンス トラクターがプライベートとしてマークされているために、クラスでのインスタンスとインスタンスのプロパティをバックするインスタンス フィールドが作成できるクラスの唯一のインスタンス。 [遅延初期化](https://msdn.microsoft.com/library/dd997286.aspx)の使用は、\_するインスタンスの作成がスレッド セーフであることを確認しますが、パフォーマンス上の理由は、インスタンス フィールドです。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスする StockTickerHub クラスで既に見たよう StockTicker のシングルトン インスタンスが StockTicker.Instance の静的プロパティから取得します。

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>ConcurrentDictionary の株価データを格納します。

    コンス トラクターによって初期化、\_いくつかのサンプルの株価データ、および GetAllStocks stocks コレクションが、株式を返します。 既に説明したよう株式のこのコレクションがさらにクライアントが呼び出すことができる、ハブ クラスにサーバーのメソッドである StockTickerHub.GetAllStocks によって返されます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    Stocks コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。 別の方法として使用できます、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトし、それを変更するときに明示的にディクショナリをロックします。

    このサンプル アプリケーションは OK StockTicker インスタンスが破棄されたときに、データが失われるとアプリケーション データをメモリに格納します。 実際のアプリケーションでは、データベースなどのバックエンド データ ストアと動作します。

    ### <a name="periodically-updating-stock-prices"></a>株価を定期的に更新

    コンス トラクターは、ランダムな単位で株価を更新するメソッドを定期的に呼び出すタイマー オブジェクトを開始します。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices は、state パラメーターで null を渡すと、タイマーによって呼び出されます。 価格を更新する前にロックを取得、 \_updateStockPricesLock オブジェクト。 別のスレッドが価格を更新中で既にと、各株式の一覧で TryUpdateStockPrice を呼び出すかどうか、コードを確認します。 TryUpdateStockPrice メソッドは、株価を変更するかどうかを決定し、これを変更する量。 株式の価格が変更された場合は、接続されているすべてのクライアントに株価の変更をブロードキャストする BroadcastStockPrice が呼び出されます。

    \_UpdatingStockPrices フラグがマーク[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)へのアクセスがスレッド セーフであることを確認します。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    実際のアプリケーションで TryUpdateStockPrice メソッドが、価格を検索する web サービスを呼び出すとこのコードでは、ランダムに変更するのに乱数ジェネレーターを使用します。

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>StockTicker クラスは、クライアントにブロードキャストできるように、SignalR のコンテキストを取得します。

    料金の変更は、StockTicker オブジェクトでは、ここに送信される、ためこれは、接続されているすべてのクライアントで、updateStockPrice メソッドを呼び出す必要があるオブジェクト。 クライアントのメソッドを呼び出すための API のあるハブ クラスでは、StockTicker ハブ クラスから派生していないと、任意のハブ オブジェクトへの参照はありません。 そのため、接続されているクライアントにブロードキャストするためには、StockTickerHub クラスの SignalR コンテキストのインスタンスを取得し、クライアントでメソッドの呼び出しに使用するに StockTicker クラスがあります。

    コンス トラクターに渡すを参照する、シングルトン クラス インスタンスを作成するときに、コードが SignalR コンテキストへの参照を取得し、コンス トラクターは、クライアントのプロパティ内に配置します。

    2 つの理由の 1 回だけ、コンテキストを取得する理由がある: 負荷の高い操作では、コンテキストを取得して、1 回取得することにより、クライアントに送信されるメッセージの目的の順序が保持されます。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    コンテキストのクライアント プロパティを取得し、StockTickerClient プロパティで、ハブ クラスの場合と同様、同じ検索するメソッドを呼び出すクライアント コードを記述することができます。 たとえば、すべてのクライアントにブロードキャストする Clients.All.updateStockPrice(stock) を記述できます。

    BroadcastStockPrice で呼び出している updateStockPrice メソッドがまだ存在しません。後で追加するクライアントで実行されるコードを記述するときにします。 Clients.All は動的で、実行時に、式が評価されることを意味しているために、updateStockPrice ここを参照できます。 このメソッドの呼び出しが実行されると、SignalR はクライアントに送信メソッド名とパラメーターの値、およびクライアントに updateStockPrice という名前のメソッドがある場合は、そのメソッドが呼び出され、それに渡されるパラメーターの値。

    Clients.All では、すべてのクライアントに送信を意味します。 SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。 詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)します。

### <a name="register-the-signalr-route"></a>SignalR のルートを登録します。

サーバーは、途中受信および SignalR への直接する URL を把握する必要があります。 追加する操作を実行し、OWIN startup クラス。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN Startup クラス**します。 クラスの名前**Startup.cs**します。
2. コードに置き換えます**Startup.cs**次です。

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

サーバー コードのセットアップが完了しました。 次のセクションでは、クライアントを設定します。

<a id="client"></a>

## <a name="set-up-the-client-code"></a>クライアント コードを設定します。

1. プロジェクト フォルダーに新しい HTML ファイルを作成し、名前*StockTicker.html*します。
2. テンプレート コードを次のコードに置き換えます。

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    HTML では、5 つの列、ヘッダー行では、5 つすべての列にまたがる単一のセルにデータ行とテーブルを作成します。 データ行は、「読み込み中...」が表示され、アプリケーションの起動時に一時的にのみ表示されます。 JavaScript コードはその行を削除して、サーバーから取得した株価データでその場所の行に追加します。

    スクリプト タグは、jQuery スクリプト ファイル、SignalR core のスクリプト ファイル、SignalR プロキシ スクリプト ファイル、および後で作成する StockTicker スクリプト ファイルを指定します。 「/Signalr ハブ」の URL を指定するには、SignalR プロキシ スクリプト ファイルが動的に生成され、StockTickerHub.GetAllStocks をここで、ハブ クラス上のメソッドをプロキシのメソッドを定義します。 使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)MapHubs メソッドの呼び出しで動的ファイルの作成を無効にするとします。
3. > [!IMPORTANT]
   > JavaScript ファイルを参照しているかどうかを確認*StockTicker.html*が正しい。 つまり、jQuery バージョン、スクリプト タグ (例では 1.10.2) では、プロジェクトの jQuery バージョンと同じであることを確認*スクリプト*フォルダー、スクリプト タグで SignalR バージョンは、SignalR と同じかどうかを確認してプロジェクトのバージョン*スクリプト*フォルダー。 必要な場合は、スクリプト タグ内のファイル名を変更します。
4. **ソリューション エクスプ ローラー**、右クリック*StockTicker.html*、 をクリックし、**スタート ページとして設定**します。
5. プロジェクト フォルダーで新しい JavaScript ファイルを作成し、名前*StockTicker.js*.
6. テンプレート コードを次のコードに置き換えます。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection は SignalR プロキシを表します。 コードでは、StockTickerHub クラスのプロキシへの参照を取得し、ティッカー変数内に配置します。 プロキシの名前は、[HubName] 属性によって設定された名前を示します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    すべての変数と関数を定義した後、ファイル内のコードの最後の行は SignalR の start 関数を呼び出すことによって、SignalR 接続を初期化します。 Start 関数が非同期的に実行し、返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)、することができる非同期操作が完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Init 関数は、ストックのテーブルを更新するサーバーが返す情報を使用して、サーバーで getAllStocks 関数を呼び出します。 既定では、あるメソッド名は pascal 形式で表記するサーバーがクライアントで camel 規約に従った大文字を使用することを確認します。 キャメル形式の規則は、オブジェクトではなく、メソッドにのみ適用されます。 たとえば、在庫を参照してください。シンボルと在庫です。価格、stock.symbol または stock.price します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    クライアントでは、pascal 形式の大文字と小文字を使用するかは HubMethodName 属性でのハブ メソッドを修飾することがまったく異なるメソッドの名前を使用する場合は、HubName 属性を持つハブ クラス自体を装飾します。

    Init メソッドでの HTML テーブルの行はストックのオブジェクトの書式設定のプロパティを呼び出し元 formatStock によってサーバーから受け取った各株オブジェクトに対して作成され、脅かすを呼び出した (の上部で定義されている*StockTicker.js*) オブジェクトのストック プロパティの値を持つ rowTemplate 変数内のプレース ホルダーを置き換えます。 結果の HTML は、株価のテーブルに追加されます。

    非同期の start 関数の完了後に実行されるコールバック関数として渡すことによって、init を呼び出します。 Init を呼び出すと、開始を呼び出した後、別の JavaScript ステートメントとして、接続の確立を完了するのには start 関数を待たずにすぐに実行があるため、関数は失敗します。 その場合は、init 関数では、サーバー接続が確立される前に、getAllStocks 関数を呼び出すとします。

    サーバー、株の価格が変更されると、接続されているクライアントで、updateStockPrice を呼び出します。 関数は、使用できるように、呼び出しに、サーバーから stockTicker プロキシのクライアントのプロパティに追加されます。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    UpdateStockPrice 関数は、テーブルの行に init 関数と同様、サーバーから受け取ったストック オブジェクトを書式設定します。 ただし、テーブルに行を追加することではなく、表内の株式の現在の行を検索し、その行を新しいものに置き換えます。

<a id="test"></a>

## <a name="test-the-application"></a>アプリケーションをテストする

1. F5 キーを押して、デバッグ モードでアプリケーションを実行します。

    ストックの表に、最初に表示されます、「読み込み中...」の行、初期の株価データを表示すると、短い遅延の後し、株式の価格を変更します。

    ![[読み込み中]](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![初期の在庫テーブル](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![サーバーからの変更を受け取るストック テーブル](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. ブラウザーのアドレス バーから URL をコピーして、1 つまたは複数の新しいブラウザー ウィンドウに貼り付けます。

    初期の株価表示は、最初のブラウザーと同じと同時に変更が行われます。
3. すべてのブラウザーを閉じるし、新しいブラウザーを開いて、同じ URL に移動します。

    StockTicker シングルトン オブジェクトは、在庫表の表示が、株式を変更し続けていることを示しています、サーバーで実行を続けています。 (図形を変更、初期テーブルのゼロは表示されません)。
4. ブラウザーを閉じます。

<a id="enablelogging"></a>

## <a name="enable-logging"></a>ログの有効化

SignalR では、トラブルシューティングで支援するために、クライアントで有効にできる組み込みのログ記録関数があります。 このセクションでは、ログ記録を有効にし、ログを知る方法を次のトランスポート メソッドの SignalR を使用して表示する例を参照してください。

- [Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされていません。
- [サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされていません。
- [フレームの永久](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。
- [長いポーリングの Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされていません。

特定の接続では、SignalR は、サーバーとクライアントの両方をサポートする最適なトランスポートの方法を選択します。

1. 開いている*StockTicker.js*し、ファイルの最後に、接続を初期化するコードの直前のログ記録を有効にするコードの行を追加します。

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. F5 キーを押してプロジェクトを実行します。
3. ブラウザーの開発者ツール ウィンドウを開きし、ログを表示するコンソールを選択します。 新しい接続の転送方法をネゴシエートする Signalr のログを表示するページを更新する必要があります。

    Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合は、Websocket は、トランスポート メソッド。

    ![IE 10 IIS 8 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合は、iframe はトランスポート メソッドです。

    ![IE 10 コンソールで、IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    Firefox の場合、コンソール ウィンドウを取得する Firebug アドインのインストールします。 Firefox 19 を Windows 8 (IIS 8) を実行する場合は、Websocket はトランスポート メソッドです。

    ![Firefox 19 IIS 8 Websocket](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Firefox 19 を Windows 7 (IIS 7.5) を実行する場合は、サーバー送信イベントが、トランスポート メソッド。

    ![Firefox 19 IIS 7.5 コンソールします。](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>インストールして、完全な StockTicker サンプルを確認してください。

StockTicker アプリケーションがインストールされている、 [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet パッケージには、最初から作成した簡略化されたバージョンより多くの機能が含まれています。 チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。 このチュートリアルの前の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN startup クラスを追加する必要があります。 この手順は、NuGet パッケージの readme.txt ファイルについて説明します。

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet パッケージをインストールします。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして、 **NuGet パッケージの管理**します。
2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**オンライン**、入力*SignalR.Sample*で、**オンライン検索**ボックス、および順にクリックします**インストール**で、 **SignalR.Sample**パッケージ。

    ![SignalR.Sample パッケージをインストールします。](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. **ソリューション エクスプ ローラー**、展開、 *SignalR.Sample* SignalR.Sample パッケージをインストールすることによって作成されたフォルダーです。
4. *SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、 をクリックし、**スタート ページとして設定**。

    > [!NOTE]
    > SignalR.Sample NuGet のインストール パッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダー。 新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーが、元のを実行する場合は、パッケージをインストールするjQueryバージョンとの同期に*StockTicker.html*ファイルを再び、最初にスクリプト タグ内の jQuery 参照を更新する必要があります。

### <a name="run-the-application"></a>アプリケーションの実行

1. F5 キーを押してアプリケーションを実行します。

    先ほど見たグリッド、だけでなくは、完全な株価表示器のアプリケーションには、同じの株価データを表示する水平方向にスクロール ウィンドウが表示されます。 最初にアプリケーションを実行して、「市場」は"closed"静的グリッドとティッカー ウィンドウ スクロールはありませんを参照してください。

    ![StockTicker 画面の開始](tutorial-server-broadcast-with-signalr/_static/image14.png)

    クリックすると**オープン市場**、**株式ティッカーの Live**を水平方向にスクロール ボックスを起動し、サーバーがランダムに株価の変更を定期的にブロードキャストする開始します。 株価たびに変更する場合に、両方、**在庫表の Live**グリッドと**株式ティッカーの Live**ボックスが更新されます。 株式の価格の変更が正、素材は緑の背景で表示され、背景が赤の素材が示すように、変更は、負の値が、場合。

    ![StockTicker アプリ市場を開く](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **閉じる市場**ボタンは、変更を停止し、ティッカー スクロールが停止し、**リセット**の料金の変更を開始する前にボタンが初期状態にすべての株価データをリセットします。 複数のブラウザー ウィンドウを開くし、同じ URL に移動する場合、各ブラウザーで同時に動的に更新される同じデータを参照してください。 クリックすると、ボタンのいずれかのブラウザーと同時に同じ方法もあります。

### <a name="live-stock-ticker-display"></a>ライブの株式相場表示

**株式ティッカーの Live**ディスプレイが CSS スタイルによって 1 行に書式設定されている div 要素の順序なしのリスト。 ティッカーは初期化され、テーブルと同様の更新: 内のプレース ホルダーを置き換えることで、 &lt;li&gt;テンプレート文字列と動的に追加する、 &lt;li&gt;要素を&lt;ul&gt;要素。 Div 内で順序付けられていない一覧の左余白を変更するため、jQuery アニメーション化する関数を使用して、実行は、スクロール

株価情報 HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

株価表示器 CSS で:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

JQuery コードがスクロールします。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>クライアントが呼び出すことができる、サーバーで追加のメソッド

StockTickerHub クラスには、クライアントが呼び出すことができる 4 つの追加のメソッドを定義します。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

OpenMarket、CloseMarket、およびリセットは、ページの上部にあるボタンへの応答で呼び出されます。 これらは、すべてのクライアントにすぐに反映される状態の変化をトリガーする 1 つのクライアントのパターンを示します。 これらの各メソッドでメソッドを呼び出し、StockTicker クラス市場の状態を変更して、新しい状態をブロードキャストし、その効果。

StockTicker クラスで MarketState 列挙型の値を返す MarketState プロパティで、市場の状態が維持されます。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

各市場の状態を変更する方法は、ロック ブロックの内部 StockTicker クラスがあるスレッド セーフのためです。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

このコードが、スレッド セーフであることを確認する、 \_MarketState プロパティをバックする marketState フィールドは、volatile としてマーク

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

BroadcastMarketStateChange と BroadcastMarketReset メソッドは、クライアントで定義されている別のメソッドを呼び出す点を既に確認した BroadcastStockPrice メソッドに似ています。

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアントの追加の関数

UpdateStockPrice 関数は、グリッドとティッカーの表示の両方を処理して、赤、緑の色をフラッシュする jQuery.Color を使用してください。

新しい関数で*SignalR.StockTicker.js*有効にして、ボタンがに基づいて無効にする市場の状態や、停止やティッカー ウィンドウ水平スクロールを開始します。 Ticker.client に複数の関数が追加されているため、 [jQuery 関数を拡張する](http://api.jquery.com/jQuery.extend/)に追加するために使用します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>接続の確立後に追加のクライアントのセットアップ

追加の作業を行うには、クライアント接続を確立した後: オープンかクローズ、適切な marketOpened または marketClosed 関数を呼び出すし、ボタンにサーバー メソッドの呼び出しをアタッチするには、市場がかどうかかを確認します。

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

サーバー メソッドがないワイヤード (有線) までボタンまで、接続が確立されると、利用される前にサーバー メソッドを呼び出すしようとすることはできません、コードを。

<a id="nextsteps"></a>

## <a name="next-steps"></a>次の手順

このチュートリアルでは、接続されているすべてのクライアント、定期的と任意のクライアントからの通知に応答の両方に、サーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学習できました。 マルチ スレッドのシングルトン インスタンスを使用して、サーバーの状態を保持するパターンも、マルチ プレーヤー オンライン ゲーム シナリオで使用こともできます。 例については、次を参照してください。 [SignalR に基づいている ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)します。

ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR によるリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)します。

SignalR 開発のより高度な概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [ASP.NET SignalR](../../index.md)
- [SignalR プロジェクト](http://signalr.net/)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。
