---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'チュートリアル: SignalR 2 と MVC 5 の概要 |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加され、チャットのビューを作成しています.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 4a4c013ff047f18f9d9b88595af7951577f3f200
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838651"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>チュートリアル: SignalR 2 と MVC 5 の概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

このチュートリアルでは、ASP.NET SignalR 2 と ASP.NET MVC 5 でリアルタイムの web アプリケーションの開発について説明します。 チュートリアルと同じのチャット アプリケーションのコードを使用して、 [SignalR 作業開始のチュートリアル](tutorial-getting-started-with-signalr.md)が、MVC 5 アプリケーションに追加する方法を示しています。

このトピックでは、次の SignalR 開発タスクを学習します。

- MVC 5 アプリケーションへの SignalR ライブラリを追加します。
- ハブとクライアントにコンテンツをプッシュする OWIN スタートアップ クラスを作成します。
- Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されている完成したチャット アプリケーションを示しています。

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [サンプルを実行します。](#run)
- [コードを確認します](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

必要条件:

- Visual Studio 2013. Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2013 Express 開発ツールを取得します。

このセクションでは、ASP.NET MVC 5 アプリケーションを作成し、SignalR ライブラリを追加、チャット アプリケーションを作成する方法を示します。

1. Visual Studio で、c# ASP.NET アプリケーションを作成するには、.NET Framework 4.5 を対象として、SignalRChat、という名前を付けます [ok] をクリックします。

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. `New ASP.NET Project`ダイアログ、および選択**MVC**、 をクリック**認証の変更**します。

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. 選択**認証なし**で、**認証の変更**ダイアログ ボックスをクリックします**OK**します。

    ![認証なし を選択します。](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > アプリケーションのさまざまな認証プロバイダーを選択した場合、`Startup.cs`クラスが作成されます。 独自に作成する必要はありません`Startup.cs`10 以下の手順でクラス。
4. クリックして**OK**で、**新しい ASP.NET プロジェクト**ダイアログ。
5. 開く、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**し、次のコマンドを実行します。 この手順では、一連のスクリプト ファイルと SignalR の機能を有効にするアセンブリ参照をプロジェクトに追加します。

    `install-package Microsoft.AspNet.SignalR`
6. **ソリューション エクスプ ローラー**、Scripts フォルダーを展開します。 SignalR のスクリプト ライブラリがプロジェクトに追加されていることに注意してください。

    ![Scripts フォルダー](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**Hubs**します。
8. 右クリックし、 **Hubs**フォルダー、をクリックして**追加 |新しい項目の**を選択、 **(Visual C#) |Web |SignalR**内のノード、**インストール済み**ペインで、 **SignalR ハブ クラス (v2)** 中央のウィンドウからという名前の新しいハブを作成および**ChatHub.cs**します。 このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。

    ![新しいハブを作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. コードに置き換えます、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Startup.cs という新しいクラスを作成します。 次に、ファイルの内容を変更します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. 編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加します。 このメソッドが戻る、**チャット**後の手順で作成するビュー。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. 右クリックし、**ビュー/ホーム**フォルダー、および選択**追加.. |。ビュー**します。
13. **ビューの追加**ダイアログ ボックスで、名前の新しいビュー**チャット**します。

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. 内容を置き換える**Chat.cshtml**を次のコード。

    > [!IMPORTANT]
    > SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加すると、パッケージ マネージャーはこのトピックに示すバージョンよりも新しい SignalR スクリプト ファイルのバージョンをインストール可能性があります。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **すべて保存**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>サンプルを実行します。

1. F5 キーを押して、デバッグ モードで、プロジェクトを実行します。
2. ブラウザーのアドレスの行の追加 **/ホーム/チャット**プロジェクトの既定のページの URL にします。 ブラウザーのインスタンスとユーザー名のプロンプトで、チャット ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. ユーザー名を入力します。
4. ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
5. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。 すべてのブラウザー インスタンスで、コメントが表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。
6. 次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。 という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。 Internet Explorer 以外のブラウザーを使用する場合は、動的もアクセスできる**hubs**ファイルを参照して直接、たとえばhttp://mywebsite/signalr/hubsします。

<a id="code"></a>

## <a name="examine-the-code"></a>コードを確認します

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。 派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**します。

**送信**メソッドがいくつかのハブの概念を示します。

- クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスするプロパティ。
- クライアントで関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

**Chat.cshtml**ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。 コードで基本的なタスクでは、自動生成されたプロキシをハブをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信への参照を作成します。

次のコードでは、ハブ プロキシへの参照を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript でサーバー クラスとそのメンバーへの参照がキャメル ケースです。 コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**します。 参照する場合、`ChatHub`クラス jquery では通常 pascal 形式で、c# の場合と大文字小文字の区別 ChatHub.cs クラス ファイルを編集します。 追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。 追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。 参照を jQuery を最後に、更新、`ChatHub`クラス。


次のコードでは、スクリプトでコールバック関数を作成する方法を示します。 サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 省略可能な呼び出し、 `htmlEncode`  ページで、スクリプト インジェクションを防止する手段として表示する前に関数を HTML に方法が表示されますが、メッセージの内容をエンコードします。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。

> [!NOTE]
> このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。 いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。

サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)
