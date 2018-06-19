---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: SignalR の頻度が高いリアルタイム 1.x |Microsoft ドキュメント
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージングには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505841"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>SignalR の頻度が高いリアルタイム 1.x
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージング、固定の率で送信される更新プログラムは、ここではこのアプリケーションの場合は、最大で 10 は、2 つ目をメッセージです。
> 
> このチュートリアルで作成したアプリケーションには、ユーザーがドラッグできる図形が表示されます。 接続されている他のすべてのブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。
> 
> このチュートリアルで導入された概念には、リアルタイムのゲーム内のアプリケーションおよびその他のシミュレーションのアプリケーションがあります。
> 
> チュートリアルのコメントは歓迎します。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com)です。


## <a name="overview"></a>概要

このチュートリアルでは、リアルタイムで他のブラウザーとオブジェクトの状態を共有しているアプリケーションを作成する方法を示します。 作成したアプリケーションには、MoveShape が呼び出されます。 ユーザーがドラッグできる; HTML Div 要素を MoveShape ページが表示されます。ドラッグすると、ユーザー、Div の新しい位置に一致する図形の位置を更新するその他のすべての接続しているクライアントに指示しは、サーバーに送信されます。

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

このチュートリアルで作成したアプリケーションは、Damian Edwards によるデモに基づいています。 このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)です。

このチュートリアルは、各発生するイベントにドラッグしたときに、その形状から SignalR メッセージを送信する方法のデモで開始されます。 接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置を更新されます。

このメソッドを使用して、アプリケーションが機能がこれはできません、推奨されるプログラミング モデルがあるためありません、クライアントとサーバーが取得集中してパンク状態メッセージと、パフォーマンスが低下するので、送信先のメッセージの数に上限を設定. クライアントで表示されているアニメーションは場所ごとにスムーズに移行するのではなく、図形は、各方法に瞬時に移動するは、不整合にもなります。 このチュートリアルの以降のセクションは、クライアントまたはサーバーがメッセージを送信する最大転送率を制限するタイマー関数を作成する方法と場所の間での図形をスムーズに移動する方法を示します。 このチュートリアルで作成したアプリケーションの最終バージョンからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)です。

このチュートリアルでは、次のセクションでは、含まれています。

- [前提条件](#prerequisites)
- [プロジェクトを作成します。](#createtheproject)
- [ASP.NET SignalR と JQuery.UI NuGet パッケージを追加します。](#nugetpackages)
- [ベースのアプリケーションを作成します。](#baseapp)
- [クライアント ループに追加します。](#clientloop)
- [サーバーのループを追加します。](#serverloop)
- [クライアントで滑らかなアニメーションを追加します。](#animation)
- [以降の手順](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、Visual Studio 2012 または Visual Studio 2010 が必要です。 Visual Studio 2010 を使用すると、プロジェクトでは、.NET Framework 4.5 ではなく、.NET Framework 4 を使用します。

Visual Studio 2012 を使用している場合は、インストールすることをお勧めしますが、 [ASP.NET および Web ツール 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)です。 この更新プログラムには、発行する、新しい機能の強化などの新機能が含まれています。 新しいテンプレートとします。

Visual Studio 2010 を使っている場合、以下のことを確認[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)がインストールされています。

<a id="createtheproject"></a>

## <a name="create-the-project"></a>プロジェクトの作成

このセクションでは、Visual Studio でプロジェクトを作成します。

1. **ファイル**ボタンをクリックし**新しいプロジェクト**です。
2. **新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**です。
3. 選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *MoveShapeDemo*、 をクリック**OK**です。

    ![新しいプロジェクトの作成](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>SignalR と JQuery.UI NuGet パッケージを追加します。

SignalR 機能は、NuGet パッケージをインストールしてプロジェクトに追加できます。 このチュートリアルは、図形をドラッグし、アニメーション化を許可する場合に、JQuery.UI パッケージを使用してもします。

1. をクリックして**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。
2. パッケージ マネージャーで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR パッケージは、依存関係として、他の NuGet パッケージの数をインストールします。 インストールが完了するとすべての SignalR を ASP.NET アプリケーションで使用するために必要なサーバーとクライアント コンポーネントがあります。
3. JQuery、および JQuery.UI パッケージをインストールするパッケージ マネージャー コンソールに次のコマンドを入力します。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>ベースのアプリケーションを作成します。

このセクションの各マウス移動イベント中に、サーバーに、図形の場所を送信するブラウザー アプリケーションを作成します。 受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。 以降のセクションでは、このアプリケーションに展開してあります。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前を付けます**MoveShapeHub**  をクリック**追加**です。
2. 新しいコードを置き換える**MoveShapeHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub`上記のクラスは、SignalR ハブの実装です。 同様、 [SignalR の概要](index.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。 ここでは、クライアントが新しいを格納するオブジェクトを送信図形を接続している他のすべてのクライアントにブロードキャストを取得し、サーバーの X および Y 座標。 SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。

    クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。 サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバーをクライアントのデータが格納される、特定のクライアントがしない独自のデータを送信できるようにします。 このメンバーは使用して、`JsonIgnore`にならないようにシリアル化され、クライアントに送信される属性です。
3. 次に、アプリケーションの起動時に、ハブを設定します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |グローバル アプリケーション クラス**です。 既定の名前を受け入れる*グローバル* をクリック**OK**です。

    ![グローバル アプリケーション クラスを追加します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. 次の追加`using`、提供された後のステートメント**を使用して**Global.asax.cs クラス内のステートメント。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. 次のコードの行を追加、 `Application_Start` SignalR の既定のルートを登録するグローバル クラスのメソッドです。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax は、ファイルは、次のようになります。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. 次に、クライアントを追加します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログで、 **Html ページ**です。 ページの適切な名前を付けます (と同様に**Default.html**) をクリック**追加**です。
7. **ソリューション エクスプ ローラー**を作成したページを右クリックし、をクリックして**スタート ページとして設定**です。
8. 次のコード スニペットでは、HTML ページで、既定のコードを置き換えます。

    > [!NOTE]
    > スクリプトを参照している一致の下の Scripts フォルダーでプロジェクトに追加されたパッケージを確認します。 Visual Studio 2010 でと JQuery、および SignalR プロジェクトに追加のバージョンでは次のバージョン番号は一致しない可能性があります。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    図形と呼ばれる赤い Div を作成、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用上の HTML および JavaScript コード`drag`図形の位置をサーバーに送信するイベントです。
9. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>クライアント ループに追加します。

すべてのマウス移動イベントで図形の場所を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからのメッセージを調整する必要があります。 Javascript を使用して`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。 このループは、「ゲーム ループ」、ドライブのすべてのゲームやその他のシミュレーションの機能を繰り返し呼び出された関数の非常に基本的な表現です。

1. 次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    上記の更新プログラムの追加、`updateServerModel`固定周波数に呼び出される関数。 この関数は、サーバーに、位置データを送信するたびに、`moved`フラグが示される位置に新しいデータを送信します。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 サーバーに送信するメッセージの数を調整するため、アニメーションは、前のセクションと同様に円滑表示されません。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>サーバーのループを追加します。

現在のアプリケーションで、サーバーからクライアントに送信されたメッセージは受信するときに多くの場合、移動します。 クライアント上で見つかりましたが、この場合は、同様の問題メッセージが必要であれば、および接続状態になるあふれたその結果よりも頻繁に送信できます。 このセクションでは、送信メッセージのレートを調整するタイマーを実装するサーバーを更新する方法について説明します。

1. 内容を置き換える`MoveShapeHub.cs`次のコード スニペットとします。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整して、送信メッセージを使用して、 `Timer` .NET framework からクラスです。

    ハブ自体が一時的なものから (は、その作成が必要なたびに)、`Broadcaster`はシングルトンとして作成されます。 (.NET 4 で導入) 限定的な初期化を使用して、作成を遅らせる必要になるまでは、タイマーが開始される前に、最初のハブ インスタンスが完全に作成されたことを確認します。

    クライアントへの呼び出し`UpdateShape`関数は、ハブの外移動`UpdateModel`メソッド、受信メッセージが受信されたときにすぐにその it が呼び出されないようにします。 25 の呼び出しの 1 秒あたりのレートでクライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラスです。

    ハブから直接クライアント メソッドを呼び出すのではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`です。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数を調整します。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>クライアントで滑らかなアニメーションを追加します。

アプリケーションがほぼ完了しましたが、1 つ以上のパフォーマンス向上、クライアントで図形を描くようにサーバー メッセージに応答を移動するときにすることもできます。 サーバーで指定された、新しい場所への図形の位置を設定するのではなく、JQuery UI ライブラリを使用`animate`の現在と新しい位置の間でスムーズに図形を移動する関数。

1. クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    上記のコードを新しいアニメーション間隔 (ここでは、100 ミリ秒) の過程で、サーバーで指定された元の場所から、図形を移動します。 新しいアニメーションの開始前に、任意図形で実行されている前のアニメーションがクリアされます。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 受信メッセージごとに 1 回設定されているのではなく、時間、動きが補間と小さいがスムーズでない他のウィンドウで、図形の移動が表示されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>以降の手順

このチュートリアルでは、クライアントとサーバー間の頻度が高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学びました。 この通信パラダイムはなどオンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR で作成された ShootR ゲーム](http://shootr.signalr.net)です。

このチュートリアルで作成した完全なアプリケーションからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)です。

SignalR 開発の概念に関する詳細についてには、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
