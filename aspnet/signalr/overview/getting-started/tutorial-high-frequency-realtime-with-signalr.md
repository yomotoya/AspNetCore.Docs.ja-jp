---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'チュートリアル: SignalR 2 によるの高頻度リアルタイム メッセージング |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高頻度のメッセージングには.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: e710980cecf9093ea9046b5790379befb5b61841
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828057"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>チュートリアル: SignalR 2 によるの高頻度リアルタイム メッセージング
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> このチュートリアルでは、ASP.NET SignalR 2 を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 この場合、固定の率で送信される更新プログラムを意味頻度の高いメッセージングこのアプリケーションの場合、最大 10 個のメッセージを 2 回目です。
> 
> このチュートリアルで作成するアプリケーションには、ユーザーがドラッグできるシェイプが表示されます。 その他のすべての接続されたブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。
> 
> このチュートリアルで導入された概念には、リアルタイムのゲームでのアプリケーションおよびその他のシミュレーション アプリケーションがあります。
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

このチュートリアルでは、リアルタイムでの他のブラウザーとオブジェクトの状態を共有するアプリケーションを作成する方法を示します。 ここで作成するアプリケーションには、MoveShape が呼び出されます。 MoveShape ページ、ユーザーがドラッグできます。 HTML Div 要素が表示されます。ユーザーが、Div をドラッグしたときは、新しい位置を送信して、サーバーは、一致するように、図形の位置を更新するその他のすべての接続されているクライアントに表示されます。

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

このチュートリアルで作成したアプリケーションは Damian Edwards によるデモに基づきます。 このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)します。

このチュートリアルは、各図形をドラッグとして起動されるイベントから SignalR メッセージを送信する方法を示すから始めます。 接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置をし更新されます。

このメソッドを使用して、アプリケーションは機能、中にこれは推奨されるプログラミング モデルでは、送信されないため、メッセージによっては、クライアントとサーバーを圧迫取得でしたし、パフォーマンスが低下するメッセージの数に上限がなくなるため. クライアントで表示されているアニメーションはそれぞれの新しい場所にスムーズに移動するのではなくの各メソッドによって、図形は瞬時に移動すると、不整合にもなります。 このチュートリアルの以降のセクションは、クライアントまたはサーバーによってメッセージが送信される最大転送率を制限するタイマー関数を作成する方法と場所の間でスムーズに図形を移動する方法を示します。 このチュートリアルで作成したアプリケーションの最終バージョンをからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)します。

このチュートリアルには、次のセクションが含まれています。

- [前提条件](#prerequisites)
- [プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加](#createtheproject2013)
- [ベースのアプリケーションを作成します。](#baseapp)
- [アプリケーションの起動時に、ハブの起動](#startup2013)
- [クライアントのループを追加します。](#clientloop)
- [サーバー ループを追加します。](#serverloop)
- [クライアントで滑らかなアニメーションを追加します。](#animation)
- [以降の手順](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、Visual Studio 2013 が必要です。

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>プロジェクトを作成し、SignalR と JQuery.UI NuGet パッケージの追加

このセクションでは、Visual Studio 2013 でプロジェクトを作成します。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR と jQuery.UI ライブラリを追加する Visual Studio 2013 を使用します。

1. Visual Studio では、ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. **新しい ASP.NET プロジェクト** ウィンドウのままに**空**選択およびクリックして**プロジェクトの作成**。

    ![空の web を作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)** します。 クラスの名前**MoveShapeHub.cs**し、プロジェクトに追加します。 この手順で作成、 **MoveShapeHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照をプロジェクトに追加します。

    > [!NOTE]
    > クリックして、プロジェクトに SignalR を追加することもできます**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。

    `install-package Microsoft.AspNet.SignalR`。 

    SignalR を追加する、コンソールを使用する場合は、SignalR を追加した後に、別のステップとして、SignalR ハブ クラスを作成します。
4. クリックして**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**します。 パッケージ マネージャー ウィンドウでは、次のコマンドを実行します。

    `Install-Package jQuery.UI.Combined`

    これは、図形をアニメーション化するのに使用する jQuery UI ライブラリをインストールします。
5. **ソリューション エクスプ ローラー**スクリプト ノードを展開します。 JQuery や jQueryUI、SignalR のスクリプト ライブラリは、プロジェクトに表示されます。

    ![スクリプト ライブラリの参照](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>ベースのアプリケーションを作成します。

このセクションで各マウス移動イベント中に、図形の場所をサーバーに送信するブラウザー アプリケーションを作成します。 受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。 以降のセクションでは、このアプリケーションに拡張します。

1. MoveShapeHub.cs クラスをまだ作成していないかどうかは**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前**MoveShapeHub**クリック**追加**します。
2. 新しいコードを置き換える**MoveShapeHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub`上記のクラスは、SignalR ハブの実装。 [SignalR の概要](tutorial-getting-started-with-signalr.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。 この場合、クライアントは新しいを格納するオブジェクトを送信する、サーバーでは、接続されている他のすべてのクライアントにブロードキャストを取得し、図形の X および Y 座標。 SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。

    クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。 サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバー クライアントのデータが格納される、特定のクライアントで独自のデータが送信されないようにします。 このメンバーを使用して、`JsonIgnore`からシリアル化され、クライアントに送信されるようにする属性。

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>アプリケーションの起動時に、ハブの起動

1. 次に、アプリケーションの起動時に、ハブへのマッピングを設定します。 SignalR 2 で、呼び出しは、OWIN startup クラスを追加することでこれは、`MapSignalR`ときに、スタートアップ クラスの`Configuration`メソッドは、OWIN の開始時に実行されます。 OWIN のスタートアップに、クラスが追加処理を使用して、`OwinStartup`アセンブリ属性。

    **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN Startup クラス**します。 クラスの名前*スタートアップ* をクリック**OK**します。
2. Startup.cs の内容を次のように変更します。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>クライアントを追加します。

1. 次に、クライアントを追加します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、 **Html ページ**します。 ページの名前を付けます**Default.html**クリック**追加**。
2. **ソリューション エクスプ ローラー**で作成したページを右クリックし、をクリックして**スタート ページとして設定**します。
3. HTML ページの既定のコードを次のコード スニペットに置き換えます。

    > [!NOTE]
    > Scripts フォルダーにプロジェクトに追加のパッケージの一致の下、スクリプトが参照を確認します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    上の HTML および JavaScript コード図形と呼ばれる赤い Div を作成し、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用`drag`図形の位置をサーバーに送信するイベントです。
4. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>クライアントのループを追加します。

すべてのマウス移動イベントに図形の位置を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからメッセージを抑制される必要があります。 使用して、javascript`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。 このループは非常に基本的な「ゲーム ループ」を繰り返し呼び出された関数でドライブのすべてのゲームやその他のシミュレーションの機能を表したものです。

1. 次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    上記の更新プログラムの追加、 `updateServerModel` 、固定周波数に呼び出される関数。 この関数は、位置データをサーバーに送信されるたびに、`moved`フラグは、送信する新しい位置データがあることを示します。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 サーバーに送信するメッセージの数は制限されますので、アニメーションは、前のセクションのようにスムーズ表示されません。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>サーバー ループを追加します。

現在のアプリケーションで、サーバーからクライアントに送信されるメッセージは受信するときに多くの場合、移動します。 クライアントで発生していたように、同様の問題を表示しますメッセージは、必要なものと、接続が溢れて結果としてより多くの場合、送信できます。 このセクションでは、送信メッセージのレートを調整するタイマーを実装するためにサーバーを更新する方法について説明します。

1. 内容を置き換える`MoveShapeHub.cs`次のコード スニペットを使用します。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整、送信メッセージを使用して、 `Timer` .NET framework からクラス。

    ハブ自体は一時的なので (必要なたびに)、作成、`Broadcaster`はシングルトンとして作成されます。 (.NET 4 で導入) 遅延初期化を使用して、作成を遅らせる必要になるまでは、タイマーを開始する前に、ハブの最初のインスタンスが完全に作成されたことを確認します。

    クライアントの呼び出し`UpdateShape`関数は、ハブからは移動し、`UpdateModel`メソッド、ことがすぐに着信メッセージを受信するたびに呼び出されないようにします。 25 の呼び出し、1 秒あたりのレートで、クライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラス。

    ハブから直接、クライアント メソッドを呼び出すことではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`します。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数は制限されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>クライアントで滑らかなアニメーションを追加します。

アプリケーションがほぼ完全には、1 つ以上向上、クライアント上の図形の動きにサーバー メッセージへの応答での移動時にすることもできます。 JQuery UI ライブラリの使用、サーバーで指定された新しい場所に図形の位置を設定するのではなく`animate`の現在および新しい位置の間でスムーズに図形を移動する関数。

1. クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    上記のコードでは、(この例では、100 ミリ秒) では、アニメーションの間隔の過程で、サーバーで指定された新しい 1 つに、古い場所から図形を移動します。 新しいアニメーションが開始される前に、図形で実行されている任意の前のアニメーションがクリアされます。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 その他のウィンドウで、図形の移動には、その動きが受信メッセージごとに 1 回設定されているのではなく、時間に補間と小さいぎくしゃくが表示されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>以降の手順

このチュートリアルでは、クライアントとサーバー間の頻度の高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学習できました。 この通信パラダイムはなど、オンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR を使って作成された ShootR ゲーム](http://shootr.signalr.net)します。

このチュートリアルで作成した完全なアプリケーションをからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)します。

SignalR 開発の概念に関する詳細については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。
