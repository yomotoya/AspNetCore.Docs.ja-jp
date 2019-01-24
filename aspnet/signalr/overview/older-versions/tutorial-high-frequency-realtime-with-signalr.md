---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: SignalR による高頻度リアルタイム メッセージング 1.x |Microsoft Docs
author: bradygaster
description: このチュートリアルでは、ASP.NET SignalR を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 高頻度のメッセージングには.
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 6df35a420a0733003808a12d065b03f08ef56dd9
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837924"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>SignalR による高頻度リアルタイム メッセージング 1.x
====================
提供者: [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このチュートリアルでは、ASP.NET SignalR を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法を示します。 この場合、固定の率で送信される更新プログラムを意味頻度の高いメッセージングこのアプリケーションの場合、最大 10 個のメッセージを 2 回目です。
> 
> このチュートリアルで作成するアプリケーションには、ユーザーがドラッグできるシェイプが表示されます。 その他のすべての接続されたブラウザーでの図形の位置は、時間指定の更新プログラムを使用して、ドラッグした図形の位置に一致するように更新されます。
> 
> このチュートリアルで導入された概念には、リアルタイムのゲームでのアプリケーションおよびその他のシミュレーション アプリケーションがあります。
> 
> このチュートリアルでコメントは、ようこそ。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com)にて投稿してください。


## <a name="overview"></a>概要

このチュートリアルでは、リアルタイムでの他のブラウザーとオブジェクトの状態を共有するアプリケーションを作成する方法を示します。 ここで作成するアプリケーションには、MoveShape が呼び出されます。 MoveShape ページ、ユーザーがドラッグできます。 HTML Div 要素が表示されます。ユーザーが、Div をドラッグしたときは、新しい位置を送信して、サーバーは、一致するように、図形の位置を更新するその他のすべての接続されているクライアントに表示されます。

![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

このチュートリアルで作成したアプリケーションは Damian Edwards によるデモに基づきます。 このデモを格納しているビデオがわかるように[ここ](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)します。

このチュートリアルは、各図形をドラッグとして起動されるイベントから SignalR メッセージを送信する方法を示すから始めます。 接続されている各クライアントは、メッセージを受信するたびに、図形のローカル バージョンの位置をし更新されます。

このメソッドを使用して、アプリケーションは機能、中にこれは推奨されるプログラミング モデルでは、送信されないため、メッセージによっては、クライアントとサーバーを圧迫取得でしたし、パフォーマンスが低下するメッセージの数に上限がなくなるため. クライアントで表示されているアニメーションはそれぞれの新しい場所にスムーズに移動するのではなくの各メソッドによって、図形は瞬時に移動すると、不整合にもなります。 このチュートリアルの以降のセクションは、クライアントまたはサーバーによってメッセージが送信される最大転送率を制限するタイマー関数を作成する方法と場所の間でスムーズに図形を移動する方法を示します。 このチュートリアルで作成したアプリケーションの最終バージョンをからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)します。

このチュートリアルには、次のセクションが含まれています。

- [必須コンポーネント](#prerequisites)
- [プロジェクトを作成します。](#createtheproject)
- [ASP.NET SignalR と JQuery.UI NuGet パッケージを追加します。](#nugetpackages)
- [ベースのアプリケーションを作成します。](#baseapp)
- [クライアントのループを追加します。](#clientloop)
- [サーバー ループを追加します。](#serverloop)
- [クライアントで滑らかなアニメーションを追加します。](#animation)
- [以降の手順](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、Visual Studio 2012 または Visual Studio 2010 が必要です。 Visual Studio 2010 を使用する場合、プロジェクトで .NET Framework 4.5 ではなく、.NET Framework 4 を使用します。

Visual Studio 2012 を使用している場合は、インストールすることをお勧めしますが、 [ASP.NET and Web Tools 2012.2 update](https://go.microsoft.com/fwlink/?LinkId=282650)します。 この更新プログラムには、発行する、新しい機能の強化などの新機能が含まれていて、新しいテンプレート。

Visual Studio 2010 があれば、以下のことを確認[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)がインストールされています。

<a id="createtheproject"></a>

## <a name="create-the-project"></a>プロジェクトの作成

このセクションでは、Visual Studio でプロジェクトを作成します。

1. **ファイル**ボタンをクリックし**新しいプロジェクト**します。
2. **新しいプロジェクト**] ダイアログ ボックスで、展開**c#** [**テンプレート**選択と**Web**します。
3. 選択、**空の ASP.NET Web アプリケーション**名では、プロジェクト テンプレートは、 *MoveShapeDemo*、 をクリック**OK**します。

    ![新しいプロジェクトを作成します。](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>SignalR と JQuery.UI NuGet パッケージを追加します。

プロジェクトに SignalR の機能を追加するには、NuGet パッケージをインストールします。 このチュートリアルは、図形をドラッグし、アニメーション化を許可する JQuery.UI パッケージを使用してもします。

1. クリックして**ツール |NuGet パッケージ マネージャー |パッケージ マネージャー コンソール**します。
2. パッケージ マネージャーで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR パッケージは、依存関係として、多くの他の NuGet パッケージをインストールします。 インストールが完了するとすべての ASP.NET アプリケーションで SignalR を使用するために必要なサーバーとクライアント コンポーネントがあります。
3. JQuery と JQuery.UI パッケージをインストールするパッケージ マネージャー コンソールに次のコマンドを入力します。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>ベースのアプリケーションを作成します。

このセクションで各マウス移動イベント中に、図形の場所をサーバーに送信するブラウザー アプリケーションを作成します。 受信すると、サーバーは、接続されている他のすべてのクライアントにこの情報をブロードキャストします。 以降のセクションでは、このアプリケーションに拡張します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加**、**クラス.**.クラスの名前**MoveShapeHub**クリック**追加**します。
2. 新しいコードを置き換える**MoveShapeHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub`上記のクラスは、SignalR ハブの実装。 [SignalR の概要](index.md)チュートリアルでは、ハブに直接に、クライアントが呼び出すメソッドがあります。 この場合、クライアントは新しいを格納するオブジェクトを送信する、サーバーでは、接続されている他のすべてのクライアントにブロードキャストを取得し、図形の X および Y 座標。 SignalR には、JSON を使用してこのオブジェクトは自動的にシリアル化します。

    クライアントに送信されるオブジェクト (`ShapeModel`) 図形の位置を格納するメンバーが含まれます。 サーバー上のオブジェクトのバージョンも含まれていますを追跡するにはメンバー クライアントのデータが格納される、特定のクライアントで独自のデータが送信されないようにします。 このメンバーを使用して、`JsonIgnore`からシリアル化され、クライアントに送信されるようにする属性。
3. 次に、アプリケーションの起動時に、ハブを設定します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |グローバル アプリケーション クラス**します。 既定の名前をそのまま使用*Global*  をクリック**OK**します。

    ![グローバル アプリケーション クラスを追加します。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. 次の追加`using`、提供された後のステートメント**を使用して**Global.asax.cs クラス内のステートメント。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. 次のコードの行を追加、 `Application_Start` SignalR の既定のルートを登録するグローバル クラスのメソッド。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax ファイルは、次のようになります。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. 次に、クライアントを追加します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、 **Html ページ**します。 ページに、適切な名前を付けます (など**Default.html**) をクリック**追加**します。
7. **ソリューション エクスプ ローラー**で作成したページを右クリックし、をクリックして**スタート ページとして設定**します。
8. HTML ページの既定のコードを次のコード スニペットに置き換えます。

    > [!NOTE]
    > Scripts フォルダーにプロジェクトに追加のパッケージの一致の下、スクリプトが参照を確認します。 Visual Studio 2010 では、JQuery、および SignalR プロジェクトに追加のバージョンには 次のバージョン番号が一致しません。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    上の HTML および JavaScript コード図形と呼ばれる赤い Div を作成し、jQuery ライブラリを使用して、図形のドラッグ動作を有効にし、図形の使用`drag`図形の位置をサーバーに送信するイベントです。
9. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>クライアントのループを追加します。

すべてのマウス移動イベントに図形の位置を送信すると、ネットワーク トラフィック量が不要なを作成するため、クライアントからメッセージを抑制される必要があります。 使用して、javascript`setInterval`固定の率でサーバーに新しい位置情報を送信するループを設定する関数。 このループは非常に基本的な「ゲーム ループ」を繰り返し呼び出された関数でドライブのすべてのゲームやその他のシミュレーションの機能を表したものです。

1. 次のコード スニペットを一致するように HTML ページ内のクライアント コードを更新します。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    上記の更新プログラムの追加、 `updateServerModel` 、固定周波数に呼び出される関数。 この関数は、位置データをサーバーに送信されるたびに、`moved`フラグは、送信する新しい位置データがあることを示します。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 サーバーに送信するメッセージの数は制限されますので、アニメーションは、前のセクションのようにスムーズ表示されません。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>サーバー ループを追加します。

現在のアプリケーションで、サーバーからクライアントに送信されるメッセージは受信するときに多くの場合、移動します。 クライアントで発生していたように、同様の問題を表示しますメッセージは、必要なものと、接続が溢れて結果としてより多くの場合、送信できます。 このセクションでは、送信メッセージのレートを調整するタイマーを実装するためにサーバーを更新する方法について説明します。

1. 内容を置き換える`MoveShapeHub.cs`次のコード スニペットを使用します。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    上記のコードを追加するクライアントの展開、`Broadcaster`クラスは、調整、送信メッセージを使用して、 `Timer` .NET framework からクラス。

    ハブ自体は一時的なので (必要なたびに)、作成、`Broadcaster`はシングルトンとして作成されます。 (.NET 4 で導入) 遅延初期化を使用して、作成を遅らせる必要になるまでは、タイマーを開始する前に、ハブの最初のインスタンスが完全に作成されたことを確認します。

    クライアントの呼び出し`UpdateShape`関数は、ハブからは移動し、`UpdateModel`メソッド、ことがすぐに着信メッセージを受信するたびに呼び出されないようにします。 25 の呼び出し、1 秒あたりのレートで、クライアントにメッセージの送信される代わりに、によって管理される、`_broadcastLoop`内からタイマー、`Broadcaster`クラス。

    ハブから直接、クライアント メソッドを呼び出すことではなく、最後に、`Broadcaster`クラスは、現在の作業ハブへの参照を取得する必要があります (`_hubContext`) を使用して、`GlobalHost`します。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 前のセクションからブラウザーで表示される相違点されませんが、クライアントに送信するメッセージの数は制限されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>クライアントで滑らかなアニメーションを追加します。

アプリケーションがほぼ完全には、1 つ以上向上、クライアント上の図形の動きにサーバー メッセージへの応答での移動時にすることもできます。 JQuery UI ライブラリの使用、サーバーで指定された新しい場所に図形の位置を設定するのではなく`animate`の現在および新しい位置の間でスムーズに図形を移動する関数。

1. クライアントの更新`updateShape`を検索するメソッドなどの次の強調表示されたコード。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    上記のコードでは、(この例では、100 ミリ秒) では、アニメーションの間隔の過程で、サーバーで指定された新しい 1 つに、古い場所から図形を移動します。 新しいアニメーションが開始される前に、図形で実行されている任意の前のアニメーションがクリアされます。
2. F5 キーを押してアプリケーションを起動します。 ページの URL をコピーして、2 番目のブラウザー ウィンドウに貼り付けます。 ブラウザー ウィンドウのいずれかで、図形をドラッグします。別のブラウザー ウィンドウ内の図形を移動する必要があります。 その他のウィンドウで、図形の移動には、その動きが受信メッセージごとに 1 回設定されているのではなく、時間に補間と小さいぎくしゃくが表示されます。

    ![アプリケーション ウィンドウ](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>以降の手順

このチュートリアルでは、クライアントとサーバー間の頻度の高いメッセージを送信する SignalR アプリケーションをプログラミングする方法を学習できました。 この通信パラダイムはなど、オンライン ゲームとその他のシミュレーションを開発するために役立ちます[SignalR を使って作成された ShootR ゲーム](http://shootr.signalr.net)します。

このチュートリアルで作成した完全なアプリケーションをからダウンロードできます[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)します。

SignalR 開発の概念に関する詳細については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)
