---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'チュートリアル: SignalR 2 使用頻度の高いリアルタイム アプリの作成 |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098591"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>チュートリアル: SignalR 2 使用頻度の高いリアルタイム アプリを作成します。

このチュートリアルでは、ASP.NET SignalR 2 を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 この場合は、「頻度の高いメッセージング」は、サーバーは、固定の率での更新を送信します。 1 秒間に最大 10 個のメッセージを送信するとします。

アプリケーションを作成するには、ユーザーがドラッグできるシェイプが表示されます。 サーバーは、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように接続されているすべてのブラウザーでの図形の位置を更新します。

このチュートリアルで導入された概念には、リアルタイムのゲームでのアプリケーションおよびその他のシミュレーション アプリケーションがあります。

このチュートリアルでしました。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * ベースのアプリケーションを作成します。
> * アプリの開始時に、ハブにマップします。
> * クライアントを追加します。
> * アプリを実行する
> * クライアントのループを追加します。
> * サーバー ループを追加します。
> * 滑らかなアニメーションを追加します。

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。

## <a name="set-up-the-project"></a>プロジェクトを設定します。

このセクションでは、Visual Studio 2017 でプロジェクトを作成します。

このセクションでは、Visual Studio 2017 を使用して、空の ASP.NET Web アプリケーションを作成し、SignalR と jQuery.UI ライブラリを追加する方法を示します。

1. Visual Studio で ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. **新しい ASP.NET Web アプリケーション - MoveShapeDemo**  ウィンドウのままに**空**を選択し、選択**OK**。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - MoveShapeDemo**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。

1. クラスの名前*MoveShapeHub*し、プロジェクトに追加します。

    この手順で作成、 *MoveShapeHub.cs*クラス ファイル。 同時に、一連のスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照を追加します。

1. 選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。

1. **パッケージ マネージャー コンソール**、このコマンドを実行します。

    ```console
    Install-Package jQuery.UI.Combined
    ```

    コマンドは、jQuery UI ライブラリをインストールします。 これを使用して、図形をアニメーション化します。

1. **ソリューション エクスプ ローラー**スクリプトを展開します。

    ![スクリプト ライブラリの参照](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    JQuery や jQueryUI、SignalR のスクリプト ライブラリは、プロジェクトに表示されます。

## <a name="create-the-base-application"></a>ベースのアプリケーションを作成します。

このセクションでは、ブラウザー アプリケーションを作成します。 アプリは、各マウス移動イベント中に、図形の場所をサーバーに送信します。 サーバーは、リアルタイムで接続されているその他のすべてのクライアントにこの情報をブロードキャストします。 以降のセクションでは、このアプリケーションの詳細について説明します。

1. 開く、 *MoveShapeHub.cs*ファイル。

1. コードに置き換えます、 *MoveShapeHub.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. ファイルを保存します。

`MoveShapeHub`クラスは、SignalR ハブの実装。 [SignalR の概要](tutorial-getting-started-with-signalr.md)チュートリアルでは、ハブには、クライアントを直接呼び出すメソッドがあります。 この場合、クライアントから送信図形をサーバーの新しい X 座標と Y を持つオブジェクトを調整します。 これらの座標は、接続されている他のすべてのクライアントにブロードキャストを取得します。 SignalR には、このオブジェクトの JSON を使用して自動的にシリアル化します。

アプリの送信、`ShapeModel`クライアント オブジェクトです。 図形の位置を格納するメンバーが存在します。 サーバー上のオブジェクトのバージョンには、どのクライアントのデータが格納されるを追跡するにはメンバーもあります。 このオブジェクトは、サーバーがそれ自体へのクライアントのデータを送信することを防ぎます。 このメンバーを使用して、`JsonIgnore`からデータをシリアル化して、それをクライアントに送信するアプリケーションを保持する属性。

## <a name="map-to-the-hub-when-app-starts"></a>アプリの開始時に、ハブにマップします。

次に、設定、ハブへのマッピングをアプリケーションの起動時にします。 SignalR 2 で OWIN startup クラスを追加するマッピングを作成します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - MoveShapeDemo**選択**インストール済み** > **Visual C#**   >  **Web**し選択**OWIN Startup クラス**します。

1. クラスの名前*スタートアップ*選択と**OK**します。

1. 既定のコードを置き換える、 *Startup.cs*このコード ファイル。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

OWIN スタートアップ クラスの呼び出し`MapSignalR`、アプリが実行される場合、`Configuration`メソッド。 OWIN のスタートアップにクラス処理を使用してアプリを追加、`OwinStartup`アセンブリ属性。

## <a name="add-the-client"></a>クライアントを追加します。

クライアントの HTML ページを追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。

1. ページの名前を付けます**既定**選択 **[ok]**。

1. **ソリューション エクスプ ローラー**、右クリック*Default.html*選択と**スタート ページとして設定**します。

1. 既定のコードを置き換える、 *Default.html*このコード ファイル。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. **ソリューション エクスプ ローラー**、展開**スクリプト**します。

    JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。

    > [!IMPORTANT]
    > パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールします。

1. プロジェクト内のスクリプト ファイルのバージョンに対応するコード ブロック内のスクリプト参照を更新します。

この HTML および JavaScript コードの作成、red`div`と呼ばれる`shape`します。 JQuery ライブラリを使用して、図形のドラッグ動作を有効にしを使用して、`drag`図形の位置をサーバーに送信するイベントです。

## <a name="run-the-app"></a>アプリを実行する

アプリを実行する se'e に作動します。 ブラウザー ウィンドウを囲む、図形をドラッグすると、図形にも、他のブラウザーで移動します。

1. ツールバーで、有効にする**スクリプトのデバッグ**し、アプリケーションをデバッグ モードで実行する [再生] ボタンを選択します。

    ![ユーザー モードをデバッグし、[再生] を選択のスクリーン ショット。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    右上隅の赤色の図形と、ブラウザー ウィンドウが開きます。

1. ページの URL をコピーします。

1. 別のブラウザーを開き、アドレス バーに URL を貼り付けます。

1. ブラウザー ウィンドウのいずれかで、図形をドラッグします。 別のブラウザー ウィンドウ内の図形に従います。

アプリケーションの中にこのメソッドを使用して関数は推奨されるプログラミング モデル。 メッセージの送信先の数に上限はありません。 その結果、クライアントとサーバー メッセージを使用した負荷がかかって取得おり、パフォーマンスが低下します。 また、アプリでは、クライアントの不整合なアニメーションを表示します。 このぎくしゃくしたアニメーションは、各メソッドによって、図形が瞬時に移動するために発生します。 図形は、それぞれの新しい場所にスムーズに移動しますを用意することをお勧めします。 次に、こうした問題を修正する方法について説明します。

## <a name="add-the-client-loop"></a>クライアントのループを追加します。

すべてのマウス移動イベントに図形の位置を送信するには、ネットワーク トラフィック量が不要なが作成されます。 アプリは、クライアントからメッセージを抑制する必要があります。

Javascript を使用して、`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。 このループは「ゲーム ループ」の基本的な表現 ゲームのすべての機能を駆動する繰り返し呼び出された関数になります。

1. クライアント コードに置き換えます、 *Default.html*このコード ファイル。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > もう一度スクリプト参照を交換する必要があります。 これらは、プロジェクト内のスクリプトのバージョンと一致する必要があります。

    この新しいコードを追加、`updateServerModel`関数。 固定の頻度にこれが呼び出されます。 関数は、位置データをサーバーに送信するたびに、`moved`フラグは、送信する新しい位置データがあることを示します。

1. アプリケーションを起動する再生ボタンを選択します。

1. ページの URL をコピーします。

1. 別のブラウザーを開き、アドレス バーに URL を貼り付けます。

1. ブラウザー ウィンドウのいずれかで、図形をドラッグします。 別のブラウザー ウィンドウ内の図形に従います。

アプリは、アニメーションをスムーズに表示されません、サーバーに送信するメッセージの数を調整するため最初でした。

## <a name="add-the-server-loop"></a>サーバー ループを追加します。

現在のアプリケーションで、サーバーからクライアントに送信されるメッセージは受信しているときに多くの場合、移動します。 このネットワーク トラフィックは、クライアントでわかるとおり、同様の問題を表示します。

アプリでは、必要なときよりもより多くの場合、メッセージを送信できます。 接続は、その結果大量に送られたなります。 このセクションでは、送信メッセージのレートを調整するタイマーを追加するサーバーを更新する方法について説明します。

1. 内容を置き換える`MoveShapeHub.cs`次のコードで。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. アプリケーションを起動する再生ボタンを選択します。

1. ページの URL をコピーします。

1. 別のブラウザーを開き、アドレス バーに URL を貼り付けます。

1. ブラウザー ウィンドウのいずれかで、図形をドラッグします。

このコードを追加するクライアントの展開、`Broadcaster`クラス。 新しいクラスを使用して、送信メッセージの調整、 `Timer` .NET framework からクラス。

については、ハブ自体が一時的なものであることをお勧めします。 これが必要になるたびに作成されます。 したがって、アプリを作成し、`Broadcaster`をシングルトンとして。 遅延初期化を遅延を使用して、`Broadcaster`の作成が必要になるまでです。 確実にタイマーを開始する前にこと、アプリが最初のハブ インスタンスを完全に作成します。

クライアントの呼び出し`UpdateShape`関数は、ハブからは移動し、`UpdateModel`メソッド。 アプリが着信メッセージを受信するたびにすぐにできなく呼び出されます。 代わりに、アプリでは、1 秒あたり 25 の呼び出しのレートでクライアントに、メッセージを送信します。 によって、プロセスが管理されている、`_broadcastLoop`内からタイマー、`Broadcaster`クラス。

ハブから直接、クライアント メソッドを呼び出すことではなく、最後に、`Broadcaster`クラスは、現在オペレーティング システムへの参照を取得する必要がある`_hubContext`ハブ。 参照を取得、`GlobalHost`します。

## <a name="add-smooth-animation"></a>滑らかなアニメーションを追加します。

アプリケーションが終了間近には、1 つ以上の向上することもできます。 アプリは、クライアント上のサーバー メッセージに応答して、図形を移動します。 サーバーで指定された新しい場所に図形の位置を設定する代わりに、JQuery UI ライブラリを使用して、`animate`関数。 現在および新しい位置の間に滑らかに図形を移動できます。

1. クライアントの更新`updateShape`メソッドで、 *Default.html*ファイルを強調表示されたコードのようになります。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. アプリケーションを起動する再生ボタンを選択します。

1. ページの URL をコピーします。

1. 別のブラウザーを開き、アドレス バーに URL を貼り付けます。

1. ブラウザー ウィンドウのいずれかで、図形をドラッグします。

他のウィンドウで、図形の移動が小さい画像が表示されます。 アプリでは、受信メッセージごとに 1 回設定されているのではなく、時間経由でその動きを補間します。

このコードは、元の場所を新しい図形を移動します。 サーバーは、アニメーションの間隔の過程で、図形の位置を示します。 この場合、100 ミリ秒です。 アプリでは、新しいアニメーションが開始される前に、図形の実行の前のアニメーションをクリアします。

## <a name="additional-resources"></a>その他の技術情報

学習した通信パラダイムはこのようなオンライン ゲームとその他のシミュレーションを開発するために便利です[SignalR を使って作成された ShootR ゲーム](https://shootr.azurewebsites.net/)します。

詳細については、SignalR は、次のリソースを参照してください。

* [SignalR プロジェクト](http://signalr.net)

* [SignalR GitHub とサンプル](https://github.com/SignalR/SignalR)

* [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>次の手順

このチュートリアルでしました。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * ベースのアプリケーションの作成
> * アプリの起動時に、ハブにマップ
> * クライアントの追加
> * アプリの実行
> * クライアントのループの追加
> * サーバー ループの追加
> * 滑らかなアニメーションを追加しました

ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [SignalR 2 とサーバー ブロードキャスト](tutorial-server-broadcast-with-signalr.md)