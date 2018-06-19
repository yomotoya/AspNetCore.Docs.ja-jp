---
uid: signalr/overview/advanced/dependency-injection
title: SignalR の依存関係の挿入 |Microsoft ドキュメント
author: MikeWasson
description: ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504181"
---
<a name="dependency-injection-in-signalr"></a>SignalR の依存関係の挿入
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


依存関係の挿入は、簡単にテストに (モック オブジェクトを使用) するか、オブジェクトの依存関係を置き換える場合、または実行時の動作を変更する、オブジェクト間の依存関係をハード コーディングされたを削除する方法です。 このチュートリアルでは、SignalR ハブで依存関係の挿入を実行する方法を示します。 SignalR で IoC コンテナーを使用する方法も示しています。 IoC コンテナーは、依存関係の挿入の一般的なフレームワークです。

## <a name="what-is-dependency-injection"></a>依存関係の挿入とは何ですか。

依存関係の挿入について熟知している場合は、このセクションをスキップします。

*依存関係の挿入*(DI) は、パターン、オブジェクトを独自の依存関係を作成できないです。 DI する動機に単純な例を次に示します。 メッセージを記録する必要があるオブジェクトがあるとします。 ログ記録のインターフェイスを定義する場合があります。

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

オブジェクトを作成できます、`ILogger`メッセージ ログに記録します。

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

これは、動作しますが、最適なデザインではありません。 置換する場合`FileLogger`別`ILogger`実装では、変更する必要がある`SomeComponent`です。 多くの他のオブジェクトによって使用されることと`FileLogger`、それらのすべてを変更する必要があります。 作成する場合または`FileLogger`シングルトンもする必要があります、アプリケーション全体での変更を加えます。

適切な方法が「注入」するには、`ILogger`オブジェクトに — たとえば、コンス トラクターの引数を使用しています。

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

これで、オブジェクトはこれを選択するために責任を負わず`ILogger`を使用します。 スイッチを作成することができます`ILogger`それに依存するオブジェクトを変更することがなく実装します。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

このパターンが呼び出された[コンス トラクター インジェクション](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)です。 別のパターンには、setter メソッドまたはプロパティを依存関係を設定する set アクセス操作子インジェクションです。

## <a name="simple-dependency-injection-in-signalr"></a>SignalR で単純な依存関係の挿入

チュートリアルのチャット アプリケーションを考えます[SignalR の概要](../getting-started/tutorial-getting-started-with-signalr.md)です。 そのアプリケーションからハブ クラスを次に示します。

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

サーバー上に送信する前にチャット メッセージを保存するとします。 この機能を抽象化するインターフェイスを定義してにインターフェイスを挿入する DI を使用する場合があります、`ChatHub`クラスです。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

唯一の問題は、SignalR アプリケーションがハブ; を直接作成していないことSignalR には、それらが作成されます。 既定では、SignalR には、パラメーターなしのコンス トラクターを持つハブ クラスが必要です。 ただし、簡単にハブ インスタンスを作成する関数を登録して DI を実行するこの関数を使用します。 関数を呼び出すことによって登録**GlobalHost.DependencyResolver.Register**です。

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

SignalR は、作成する必要があるたびに、この匿名の関数が呼び出すので、`ChatHub`インスタンス。

## <a name="ioc-containers"></a>IoC コンテナー

上記のコードは、単純な場合に適しています。 これを記述する必要があります。

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

多くの依存関係を持つ複雑なアプリケーションでは、多くの「配線」このコードを記述する必要があります。 依存関係が入れ子になった場合に特に、このコードは、維持するために難しくなります。 また、単体テストに困難です。

1 つのソリューションでは、IoC コンテナーを使用します。 IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。型のコンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。 コンテナーは、依存関係を自動的に判別されます。 多くの IoC コンテナーを使用すると、オブジェクトの有効期間、スコープなどを制御できます。

> [!NOTE]
> "IoC"が「コントロールの反転」の略フレームワークがアプリケーション コードを呼び出す、一般的なパターンはします。 IoC コンテナーは、通常の制御フローの「反転」に、オブジェクトに構築します。


## <a name="using-ioc-containers-in-signalr"></a>SignalR で IoC コンテナーの使用

チャット アプリケーションは、IoC コンテナーから利点を得るが簡単すぎます可能性があります。 代わりに、見てみましょう、 [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)サンプルです。

StockTicker サンプルでは、次の 2 つの主要なクラスを定義します。

- `StockTickerHub`:、ハブ クラスのクライアント接続を管理します。
- `StockTicker`: 株価を保持し、定期的に更新するシングルトンです。

`StockTickerHub`参照を保持している、`StockTicker`シングルトン、中に`StockTicker`への参照を保持している、 **IHubConnectionContext**の`StockTickerHub`です。 このインターフェイスを使用して通信するために`StockTickerHub`インスタンス。 (詳細については、次を参照してください[サーバーは、ASP.NET SignalR でブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。

IoC コンテナー ビットこれらの依存関係の解決に使用できます。 最初に、簡略化してみましょう、`StockTickerHub`と`StockTicker`クラスです。 次のコードでをコメント アウトしました部分しない必要があります。

パラメーターなしのコンス トラクターからの削除`StockTickerHub`です。 代わりに、ハブを作成するのに DI を常に使用します。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

StockTicker、シングルトン インスタンスを削除します。 その後、StockTicker 有効期間を制御するのに IoC コンテナーが使用されます。 パブリック コンス トラクターは、また、ください。

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

次に、インターフェイスを作成することで、コードをリファクタリングできます`StockTicker`です。 このインターフェイスを使用して、分離、`StockTickerHub`から、`StockTicker`クラスです。

Visual Studio でこの種類のリファクタリング簡単です。 StockTicker.cs ファイルを開きを右クリックし、`StockTicker`クラス宣言、および選択**リファクター**しています.**インターフェイスの抽出**です。

![](dependency-injection/_static/image1.png)

**インターフェイスの抽出**ダイアログ ボックスで、をクリックして**すべて選択**です。 他の既定値のままにします。 **[OK]** をクリックします。

![](dependency-injection/_static/image2.png)

Visual Studio という名前の新しいインターフェイスを作成する`IStockTicker`、変更のほか`StockTicker`から派生する`IStockTicker`です。

IStockTicker.cs ファイルを開き、変更するためのインターフェイス**パブリック**です。

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

`StockTickerHub`クラスを 2 つのインスタンスを変更、`StockTicker`に`IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

作成する、`IStockTicker`インターフェイスは必ずしも必要はありませんが、アプリケーションのコンポーネント間の結合を削減する DI どう役立つかを表示する必要です。

## <a name="add-the-ninject-library"></a>Ninject ライブラリを追加します。

.NET の多くのオープン ソース IoC コンテナーがあります。 このチュートリアルを使用します[Ninject](http://www.ninject.org/)です。 (その他の一般的なライブラリを含む[城 Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Unity](https://github.com/unitycontainer/unity)、および[StructureMap](http://docs.structuremap.net).)

NuGet パッケージ マネージャーを使用して、インストール、 [Ninject ライブラリ](https://nuget.org/packages/Ninject/3.0.1.10)です。 Visual Studio から、**ツール**メニュー選択**ライブラリ パッケージ マネージャー** | **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>SignalR の依存関係競合回避モジュールを交換します。

SignalR 内 Ninject を使用するから派生するクラスを作成**DefaultDependencyResolver**です。

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

このクラスは、オーバーライド、 **GetService**と**GetServices**のメソッド**DefaultDependencyResolver**です。 SignalR ハブ インスタンスの SignalR によって内部的に使用されるさまざまなサービスなど、実行時にさまざまなオブジェクトを作成するこれらのメソッドを呼び出します。

- **GetService**メソッドは、型の 1 つのインスタンスを作成します。 呼び出す Ninject カーネルのこのメソッドをオーバーライド**TryGet**メソッドです。 そのメソッドが null を返す場合は、既定のリゾルバーにフォールバックします。
- **GetServices**メソッドは、指定した型のオブジェクトのコレクションを作成します。 既定の競合回避モジュールからの結果に Ninject から結果を連結するには、このメソッドをオーバーライドします。

## <a name="configure-ninject-bindings"></a>Ninject バインディングを構成します。

宣言型バインディングに Ninject を使用します。

アプリケーションの Startup.cs クラスを開きます (いるいずれかの手動で作成したパッケージの指示に従って`readme.txt`認証をプロジェクトに追加することによって作成されたか)。 `Startup.Configuration`メソッド、Ninject 呼び出す Ninject コンテナーを作成、*カーネル*です。

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

このカスタム依存関係競合回避モジュールのインスタンスを作成します。

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

バインディングを作成する`IStockTicker`次のようにします。

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

このコードは、次の 2 つというメッセージがします。 最初に、アプリケーションに必要なときに、 `IStockTicker`、カーネルがのインスタンスを作成する必要があります`StockTicker`です。 2 番目、`StockTicker`クラスは、シングルトン オブジェクトとして作成する必要があります。 Ninject は、オブジェクトの 1 つのインスタンスを作成し、各要求に対して同じインスタンスを返します。

バインディングを作成する**IHubConnectionContext**次のようにします。

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

このコード creatres を返す匿名関数、 **IHubConnection**です。 **WhenInjectedInto**メソッドに指示を作成するときにのみ、この関数を使用する Ninject`IStockTicker`インスタンス。 その理由は、SignalR が作成される**IHubConnectionContext**インスタンスを内部的には、SignalR がそれらを作成する方法をオーバーライドする必要はないです。 この関数にのみ適用されます、`StockTicker`クラスです。

依存関係リゾルバーを渡す、 **MapSignalR**ハブの構成を追加して、メソッド。

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

新しいパラメーターを使用して、サンプルのスタートアップ クラスで Startup.ConfigureSignalR メソッドを更新します。

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

SignalR で指定された競合回避モジュールを使用するようになりました**MapSignalR**既定の競合回避モジュールの代わりにします。

完全なコードの一覧を次に示します`Startup.Configuration`です。

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Visual Studio で StockTicker アプリケーションを実行するには、f5 キーを押します。 ブラウザー ウィンドウに移動`http://localhost:*port*/SignalR.Sample/StockTicker.html`です。

![](dependency-injection/_static/image3.png)

アプリケーションには、正確に前に、のと同じ機能があります。 (詳細については、次を参照してください[サーバーは、ASP.NET SignalR でブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。動作を変更していません。行ったコードのテスト、保守、および発展しやすくします。
