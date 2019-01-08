---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'チュートリアル: SignalR 2 と MVC 5 のリアルタイムのチャット |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR は、MVC 5 アプリケーションを追加します。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098605"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>チュートリアル: SignalR 2 と MVC 5 のリアルタイムのチャット

このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。

このチュートリアルでしました。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * サンプルを実行します。
> * コードを確認します

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。

## <a name="set-up-the-project"></a>プロジェクトを設定します。

このセクションでは、Visual Studio 2017 と SignalR 2 を使用して、空の ASP.NET MVC 5 アプリケーションを作成し、SignalR ライブラリの追加、チャット アプリケーションを作成する方法を示します。

1. Visual Studio で、c# ASP.NET アプリケーションを作成するには、.NET Framework 4.5 を対象として、SignalRChat、という名前を付けます [ok] をクリックします。

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. **新しい ASP.NET Web アプリケーション - SignalRMvcChat**を選択します**MVC**選び**認証の変更**します。

1. **認証の変更**を選択します**認証なし** をクリック**OK**します。

    ![認証なし を選択します。](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. **新しい ASP.NET Web アプリケーション - SignalRMvcChat**、 **OK**。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。

1. **新しい項目の追加 - SignalRChat**、**インストール済み** > **Visual C#**   >  **Web**  > **SignalR**選び**SignalR ハブ クラス (v2)** します。

1. クラスの名前*ChatHub*し、プロジェクトに追加します。

    この手順で作成、 *ChatHub.cs*クラス ファイルとスクリプト ファイルとプロジェクトに SignalR をサポートするアセンブリ参照のセットを追加します。

1. 新しいコードを置き換える*ChatHub.cs*次のコードでクラス ファイル。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。

1. 新しいクラスの名前*スタートアップ*し、プロジェクトに追加します。

1. コードに置き換えます、 *Startup.cs*次のコードでクラス ファイル。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. **ソリューション エクスプ ローラー**、**コント ローラー** > **HomeController.cs**します。

1. このメソッドを追加、 *HomeController.cs*します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    このメソッドが戻る、**チャット**後の手順で作成するビュー。

1. **ソリューション エクスプ ローラー**、右クリック**ビュー** > **ホーム**、選び**追加** >   **ビュー**します。

1. **ビューの追加**、新しいビューを指定して**チャット**選択**追加**します。

1. 内容を置き換える**Chat.cshtml**次のコードで。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. **ソリューション エクスプ ローラー**、展開**スクリプト**します。

    JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。

    > [!IMPORTANT]
    > パッケージ マネージャーは、以降のバージョンの SignalR スクリプトをインストールがある可能性があります。

1. コード ブロック内のスクリプト参照がプロジェクト内のスクリプト ファイルのバージョンに対応することを確認します。

    スクリプトの元のコード ブロックからの参照。

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. 一致しない場合は、更新、 *.cshtml*ファイル。

1. メニュー バーから選択**ファイル** > **すべて保存**します。

## <a name="run-the-sample"></a>サンプルを実行します。

1. ツールバーで、有効にする**スクリプトのデバッグ**し、サンプルをデバッグ モードで実行する [再生] ボタンを選択します。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. ブラウザーが開いたら、チャット、id の名前を入力します。

1. ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。

1. 各ブラウザーでは、一意の名前を入力します。

1. ここで、追加、コメントを**送信**します。 他のブラウザーでは繰り返しません。 コメントは、リアルタイムで表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。

    次の 3 つのさまざまなブラウザーでのチャット アプリケーションの実行方法を参照してください。 Tom、Anand、Susan は、メッセージを送信して、すべてのブラウザーはリアルタイムで更新します。

    ![次の 3 つのすべてのブラウザーが同じチャットの履歴を表示します。](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 という名前のスクリプト ファイルがある*hubs* SignalR ライブラリは、実行時に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![スクリプト ドキュメント ノード内の自動生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>コードを確認します

SignalR のチャット アプリケーションでは、2 つの基本的な SignalR 開発タスクを示しています。 ハブを作成する方法を示します。 サーバーは、メインの調整オブジェクトとしてそのハブを使用します。 ハブは、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs-in-the-chathubcs"></a>SignalR ハブで、ChatHub.cs

コード サンプルで、`ChatHub`クラスから派生、`Microsoft.AspNet.SignalR.Hub`クラス。 派生する、`Hub`クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、`ChatHub.Send`新しいメッセージを送信する方法。 ハブに送信メッセージのすべてのクライアントを呼び出して`Clients.All.addNewMessageToPage`します。

`Send`メソッドがいくつかのハブの概念を示します。

* クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。

* 使用して、`Microsoft.AspNet.SignalR.Hub.Clients`この hub に接続されているすべてのクライアントと通信する動的プロパティ。

* クライアントで関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR と jQuery Chat.cshtml

*Chat.cshtml*ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。  コードは、多くの重要なタスクを実行します。 ハブの自動生成されたプロキシへの参照を作成しをクライアントにコンテンツをプッシュするサーバーを呼び出すことができ、ハブにメッセージを送信への接続を開始する関数を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript では、サーバー クラスとそのメンバーへの参照はキャメル ケースでは。 コード サンプルの参照、 C# `ChatHub`として JavaScript でクラス`chatHub`します。

このコード ブロックでは、スクリプト内のコールバック関数を作成します。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 省略可能な呼び出し、`htmlEncode`関数を HTML に方法が表示されますが、ページに表示する前に、メッセージの内容をエンコードします。 スクリプト インジェクションを防止する方法になります。

このコードは、ハブとの接続を開きます。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> このアプローチによりイベント ハンドラーが実行される前に、接続を確立するようになります。

コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。

## <a name="additional-resources"></a>その他の技術情報

詳細については、SignalR は、次のリソースを参照してください。

* [SignalR プロジェクト](http://signalr.net)

* [SignalR GitHub とサンプル](https://github.com/SignalR/SignalR)

* [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>次の手順

このチュートリアルでしました。

> [!div class="checklist"]
> * プロジェクトを設定します。
> * サンプルの実行
> * コードを調べる

ASP.NET SignalR 2 を使用して、頻度の高いメッセージング機能を提供する web アプリケーションを作成する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [頻度の高いメッセージングを使用した web アプリ](tutorial-high-frequency-realtime-with-signalr.md)