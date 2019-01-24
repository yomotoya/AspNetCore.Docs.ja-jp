---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR 2 とリアルタイムのチャット |Microsoft Docs'
author: bradygaster
description: このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR は、空の ASP.NET web アプリケーションに追加します。
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836793"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>チュートリアル: SignalR 2 とリアルタイムのチャット

このチュートリアルでは、SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * サンプルを実行します。
> * コードを確認します

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。

## <a name="set-up-the-project"></a>プロジェクトを設定します。

SignalR を追加するこのセクションでは、Visual Studio 2017 と SignalR 2 を使用して、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。

1. Visual Studio で ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)

1. **新しい ASP.NET プロジェクト - SignalRChat**  ウィンドウのままに**空**を選択し、選択**OK**。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - SignalRChat**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。

1. クラスの名前*ChatHub*し、プロジェクトに追加します。

    この手順で作成、 *ChatHub.cs*クラス ファイルとスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照のセットを追加します。

1. 新しいコードを置き換える*ChatHub.cs*次のコードでクラス ファイル。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - SignalRChat**選択**インストール済み** > **Visual C#**   >  **Web**し選択**OWIN Startup クラス**します。

1. クラスの名前*スタートアップ*し、プロジェクトに追加します。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。

1. 名前を新しいページ*インデックス*を選択し、 **OK**します。

1. **ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、選択**スタート ページとして設定**します。

1. このコードでは、HTML ページの既定のコードを置き換えます。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. **ソリューション エクスプ ローラー**、展開**スクリプト**します。

    JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。

    > [!IMPORTANT]
    > パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールがある可能性があります。

1. コード ブロック内のスクリプト参照がプロジェクト内のスクリプト ファイルのバージョンに対応することを確認します。

    スクリプトの元のコード ブロックからの参照。
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. 一致しない場合は、更新、 *.html*ファイル。

1. メニュー バーから選択**ファイル** > **すべて保存**します。

## <a name="run-the-sample"></a>サンプルを実行します。

1. ツールバーで、有効にする**スクリプトのデバッグ**し、サンプルをデバッグ モードで実行する [再生] ボタンを選択します。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image3.png)

1. ブラウザーが開いたら、チャット、id の名前を入力します。

1. ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。

1. 各ブラウザーでは、一意の名前を入力します。

1. ここで、追加、コメントを**送信**します。 他のブラウザーでは繰り返しません。 コメントは、リアルタイムで表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。

    次の 3 つのさまざまなブラウザーでのチャット アプリケーションの実行方法を参照してください。 Tom、Anand、Susan は、メッセージを送信して、すべてのブラウザーはリアルタイムで更新します。

    ![次の 3 つのすべてのブラウザーが同じチャットの履歴を表示します。](tutorial-getting-started-with-signalr/_static/image4.png)

1. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 という名前のスクリプト ファイルがある*hubs* SignalR ライブラリは、実行時に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![スクリプト ドキュメント ノード内の自動生成されたハブのスクリプト](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>コードを確認します

SignalRChat アプリケーションでは、2 つの基本的な SignalR 開発タスクを示します。 ハブを作成する方法を示します。 サーバーは、メインの調整オブジェクトとしてそのハブを使用します。 ハブは、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs-in-the-chathubcs"></a>SignalR ハブで、ChatHub.cs

上記のコード サンプルで、`ChatHub`クラスから派生、`Microsoft.AspNet.SignalR.Hub`クラス。 派生する、`Hub`クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドを使用できます。

クライアントが呼び出すチャットのコードで、`ChatHub.Send`新しいメッセージを送信する方法。 ハブのメッセージを送信しすべてのクライアントを呼び出して`Clients.All.broadcastMessage`します。

`Send`メソッドがいくつかのハブの概念を示します。

* クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。

* 使用して、`Microsoft.AspNet.SignalR.Hub.Clients`この hub に接続されているすべてのクライアントと通信する動的プロパティ。

* クライアントで関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR と、index.html で jQuery

*Index.html*ページ コード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。 コードは、多くの重要なタスクを実行します。 ハブを参照するプロキシの宣言、ことをクライアントにコンテンツをプッシュするサーバーを呼び出すことができ、ハブにメッセージを送信への接続を開始関数を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript でサーバー クラスとそのメンバーへの参照は camelCase が。 コード サンプルの参照、 C# *ChatHub*として JavaScript でクラス`chatHub`します。

このコード ブロックでは、スクリプト内のコールバック関数を作成します。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 2 つの行を HTML エンコード、コンテンツを表示する前に省略可能で、スクリプト インジェクションを防止する優れた方法を表示します。

このコードは、ハブとの接続を開きます。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> このアプローチにより、イベント ハンドラーが実行される前に、コードでの接続を確立するようになります。

コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。

## <a name="get-the-code"></a>コードを取得する

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>その他の技術情報

詳細については、SignalR は、次のリソースを参照してください。

* [SignalR プロジェクト](http://signalr.net)

* [SignalR Github とサンプル](https://github.com/SignalR/SignalR)

* [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>次の手順

このチュートリアルではします。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * サンプルの実行
> * コードを調べる

SignalR と MVC 5 を使用する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [SignalR 2 と MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)