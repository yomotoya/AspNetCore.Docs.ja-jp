---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: "チュートリアル: 頻度が高いリアルタイム signalr 2 |Microsoft ドキュメント"
author: pfletcher
description: "このチュートリアルでは、ASP.NET SignalR を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージングには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5af7289392c18d58de11249c75e539c9e08954be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>チュートリアル: 頻度が高いリアルタイム signalr 2
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> このチュートリアルでは、ASP.NET SignalR 2 を使用して、頻度が高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高周波数のメッセージング、固定の率で送信される更新プログラムは、ここではこのアプリケーションの場合は、最大で 10 は、2 つ目をメッセージです。
> 
> このチュートリアルで作成したアプリケーションには、ユーザーがドラッグできる図形が表示されます。 接続されている他のすべてのブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。
> 
> このチュートリアルで導入された概念には、リアルタイムのゲーム内のアプリケーションおよびその他のシミュレーションのアプリケーションがあります。
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

このチュートリアルでは、リアルタイムで他のブラウザーとオブジェクトの状態を共有しているアプリケーションを作成する方法を示します。 作成したアプリケーションには、MoveShape が呼び出されます。 ユーザーがドラッグできる; HTML Div 要素を MoveShape ページが表示されます。ドラッグすると、ユーザー、Div の新しい位置に一致する図形の位置を更新するその他のすべての接続しているクライアントに指示しは、サーバーに送信されます。

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

このチュートリアルで作成したアプリケーションは、Damian Edwards によるデモに基づいています。 このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)です。

このチュートリアルは、各発生するイベントにドラッグしたときに、その形状から SignalR メッセージを送信する方法のデモで開始されます。 接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置を更新されます。

このメソッドを使用して、アプリケーションが機能がこれはできません、推奨されるプログラミング モデルがあるためありません、クライアントとサーバーが取得集中してパンク状態メッセージと、パフォーマンスが低下するので、送信先のメッセージの数に上限を設定. クライアントで表示されているアニメーションは場所ごとにスムーズに移行するのではなく、図形は、各方法に瞬時に移動するは、不整合にもなります。 このチュートリアルの以降のセクションは、クライアントまたはサーバーがメッセージを送信する最大転送率を制限するタイマー関数を作成する方法と場所の間での図形をスムーズに移動する方法を示します。 このチュートリアルで作成したアプリケーションの最終バージョンからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)です。

このチュートリアルでは、次のセクションでは、含まれています。

- [前提条件](#prerequisites)
- [プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加](#createtheproject2013)
- [ベースのアプリケーションを作成します。](#baseapp)
- [アプリケーションの起動時に、ハブの開始](#startup2013)
- [クライアント ループに追加します。](#clientloop)
- [サーバーのループを追加します。](#serverloop)
- [クライアントで滑らかなアニメーションを追加します。](#animation)
- [以降の手順](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、Visual Studio 2013 が必要です。

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加

このセクションで Visual Studio 2013 で、プロジェクトが作成されます。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR および jQuery.UI ライブラリを追加する Visual Studio 2013 を使用します。

1. Visual Studio では、ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. **新しい ASP.NET プロジェクト** ウィンドウのままにして**空**を選択し、をクリックして**プロジェクトの作成**です。

    ![空の web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)**です。 クラスの名前を付けます**MoveShapeHub.cs**し、プロジェクトに追加します。 この手順で作成、 **MoveShapeHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照がプロジェクトに追加します。

    > [!NOTE]
    > クリックして、プロジェクトに SignalR を追加することも**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。

    `install-package Microsoft.AspNet.SignalR`。 

    コンソールを使用して SignalR を追加する場合は、SignalR を追加した後に、別のステップとして SignalR ハブ クラスを作成します。
4. をクリックして**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。 パッケージ マネージャー ウィンドウで、次のコマンドを実行します。

    `Install-Package jQuery.UI.Combined`

    これは、図形をアニメーション化するときに使用される、jQuery UI ライブラリをインストールします。
5. **ソリューション エクスプ ローラー**スクリプト ノードを展開します。 JQuery、jQueryUI、および SignalR のスクリプト ライブラリは、プロジェクトに表示されます。

    ![スクリプト ライブラリの参照](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>ベースのアプリケーションを作成します。

このセクションの各マウス移動イベント中に、サーバーに、図形の場所を送信するブラウザー アプリケーションを作成します。 受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。 以降のセクションでは、このアプリケーションに展開してあります。

1. MoveShapeHub.cs クラスをまだ作成していないかどうかは**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前を付けます**MoveShapeHub**  をクリック**追加**です。
2. 新しいコードを置き換える**MoveShapeHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub`上記のクラスは、SignalR ハブの実装です。 同様、 [SignalR の概要](tutorial-getting-started-with-signalr.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。 ここでは、クライアントが新しいを格納するオブジェクトを送信図形を接続している他のすべてのクライアントにブロードキャストを取得し、サーバーの X および Y 座標。 SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。

    クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。 サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバーをクライアントのデータが格納される、特定のクライアントがしない独自のデータを送信できるようにします。 このメンバーは使用して、`JsonIgnore`にならないようにシリアル化され、クライアントに送信される属性です。

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>アプリケーションの起動時に、ハブの開始

1. 次に、アプリケーションの起動時に、ハブへのマッピングを設定します。 SignalR 2 では、呼び出しは、OWIN スタートアップ クラスを追加することによって実行`MapSignalR`ときにスタートアップ クラス`Configuration`OWIN の開始時に、メソッドを実行します。 OWIN の起動に、クラスが追加処理を使用して、`OwinStartup`アセンブリの属性です。

    **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN 起動クラス**です。 クラスに名前を*スタートアップ* をクリック**OK**です。
2. Startup.cs の内容を次のように変更します。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>クライアントを追加します。

1. 次に、クライアントを追加します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログで、 **Html ページ**です。 ページの名前を付けます**Default.html**  をクリック**追加**です。
2. **ソリューション エクスプ ローラー**を作成したページを右クリックし、をクリックして**スタート ページとして設定**です。
3. 次のコード スニペットでは、HTML ページで、既定のコードを置き換えます。

    > [!NOTE]
    > スクリプトを参照している一致の下の Scripts フォルダーでプロジェクトに追加されたパッケージを確認します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    図形と呼ばれる赤い Div を作成、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用上の HTML および JavaScript コード`drag`図形の位置をサーバーに送信するイベントです。
4. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>クライアント ループに追加します。

すべてのマウス移動イベントで図形の場所を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからのメッセージを調整する必要があります。 Javascript を使用して`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。 このループは、「ゲーム ループ」、ドライブのすべてのゲームやその他のシミュレーションの機能を繰り返し呼び出された関数の非常に基本的な表現です。

1. 次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    上記の更新プログラムの追加、`updateServerModel`固定周波数に呼び出される関数。 この関数は、サーバーに、位置データを送信するたびに、`moved`フラグが示される位置に新しいデータを送信します。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 サーバーに送信するメッセージの数を調整するため、アニメーションは、前のセクションと同様に円滑表示されません。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>サーバーのループを追加します。

現在のアプリケーションで、サーバーからクライアントに送信されたメッセージは受信するときに多くの場合、移動します。 クライアント上で見つかりましたが、この場合は、同様の問題メッセージが必要であれば、および接続状態になるあふれたその結果よりも頻繁に送信できます。 このセクションでは、送信メッセージのレートを調整するタイマーを実装するサーバーを更新する方法について説明します。

1. 内容を置き換える`MoveShapeHub.cs`次のコード スニペットとします。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整して、送信メッセージを使用して、 `Timer` .NET framework からクラスです。

    ハブ自体が一時的なものから (は、その作成が必要なたびに)、`Broadcaster`はシングルトンとして作成されます。 (.NET 4 で導入) 限定的な初期化を使用して、作成を遅らせる必要になるまでは、タイマーが開始される前に、最初のハブ インスタンスが完全に作成されたことを確認します。

    クライアントへの呼び出し`UpdateShape`関数は、ハブの外移動`UpdateModel`メソッド、受信メッセージが受信されたときにすぐにその it が呼び出されないようにします。 25 の呼び出しの 1 秒あたりのレートでクライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラスです。

    ハブから直接クライアント メソッドを呼び出すのではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`です。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数を調整します。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>クライアントで滑らかなアニメーションを追加します。

アプリケーションがほぼ完了しましたが、1 つ以上のパフォーマンス向上、クライアントで図形を描くようにサーバー メッセージに応答を移動するときにすることもできます。 サーバーで指定された、新しい場所への図形の位置を設定するのではなく、JQuery UI ライブラリを使用`animate`の現在と新しい位置の間でスムーズに図形を移動する関数。

1. クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    上記のコードを新しいアニメーション間隔 (ここでは、100 ミリ秒) の過程で、サーバーで指定された元の場所から、図形を移動します。 新しいアニメーションの開始前に、任意図形で実行されている前のアニメーションがクリアされます。
2. F5 キーを押して、アプリケーションを起動します。 ページの URL をコピーし、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザーのウィンドウのいずれかで図形をドラッグしてください。他のブラウザー ウィンドウで、図形を移動する必要があります。 受信メッセージごとに 1 回設定されているのではなく、時間、動きが補間と小さいがスムーズでない他のウィンドウで、図形の移動が表示されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>以降の手順

このチュートリアルでは、クライアントとサーバー間の頻度が高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学びました。 この通信パラダイムはなどオンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR で作成された ShootR ゲーム](http://shootr.signalr.net)です。

このチュートリアルで作成した完全なアプリケーションからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)です。

SignalR 開発の概念に関する詳細についてには、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。 Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)です。
