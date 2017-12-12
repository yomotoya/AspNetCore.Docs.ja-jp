---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "チュートリアル: SignalR 2 と MVC 5 の概要 |Microsoft ドキュメント"
author: pfletcher
description: "このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加され、チャット ビューを作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 96a6f6446b26d96b2bcffe4354375ab9c444bbbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>チュートリアル: SignalR 2 と MVC 5 の概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加し、送信、メッセージを表示するチャット ビューを作成します。 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

このチュートリアルでは、ASP.NET SignalR 2 と ASP.NET MVC 5 のリアルタイムの web アプリケーションの開発に紹介します。 チュートリアル、チャット アプリケーションと同じコードを使用して、[チュートリアル SignalR が入門](tutorial-getting-started-with-signalr.md)MVC 5 アプリケーションに追加する方法を示していますが、します。

このトピックでは、次の SignalR 開発タスクを学習します。

- MVC 5 アプリケーションへの SignalR ライブラリを追加します。
- ハブとクライアントにコンテンツをプッシュする OWIN スタートアップ クラスを作成しています。
- Web ページでは、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブからの更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されている完了したチャット アプリケーションを示します。

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [このサンプルを実行します。](#run)
- [コードを調べます](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

必要条件:

- Visual Studio 2013。 Visual Studio がない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)空き Visual Studio 2013 Express 開発ツールを取得します。

このセクションでは、ASP.NET MVC 5 アプリケーションを作成し、SignalR ライブラリを追加し、チャット アプリケーションを作成する方法を示します。

1. Visual Studio で、.NET Framework 4.5 を対象とする c# ASP.NET アプリケーションを作成し、SignalRChat、という名前を [ok] をクリックします。

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. `New ASP.NET Project`ダイアログ、および選択**MVC**、 をクリック**認証の変更**です。

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. 選択**認証なし**で、**認証の変更**ダイアログ ボックスをクリック**OK**です。

    ![認証なし を選択します。](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > アプリケーションの異なる認証プロバイダーを選択する場合、`Startup.cs`クラスが作成されます。 独自に作成する必要はありません`Startup.cs`10 以下の手順でクラスです。
4. をクリックして**OK**で、**新しい ASP.NET プロジェクト**ダイアログ。
5. 開く、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**し、次のコマンドを実行します。 この手順では、一連のスクリプト ファイルと SignalR 機能を有効にするアセンブリ参照をプロジェクトに追加します。

    `install-package Microsoft.AspNet.SignalR`
6. **ソリューション エクスプ ローラー**、Scripts フォルダーを展開します。 SignalR のスクリプト ライブラリがプロジェクトに追加されたことに注意してください。

    ![Scripts フォルダー](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**ハブ**です。
8. 右クリックし、**ハブ**フォルダーで、をクリックして**追加 |新しい項目の**、select、 **Visual c# |Web |SignalR**内のノード、**インストール**ペインで、 **SignalR ハブ クラス (v2)**中央のウィンドウからという名前の新しいハブの作成と**ChatHub.cs**です。 このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。

    ![新しいハブを作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. コードで置き換え、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Startup.cs という新しいクラスを作成します。 ファイルの内容を次のように変更します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. 編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加するとします。 このメソッドが戻る、**チャット**後の手順で作成するビュー。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. 右クリックし、**ビュー/ホーム**フォルダー、および選択**追加します.. |。ビュー**です。
13. **ビューの追加**ダイアログ ボックスで、名前、新しいビュー**チャット**です。

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. 内容を置き換える**Chat.cshtml**を次のコード。

    > [!IMPORTANT]
    > SignalR と他のスクリプト ライブラリを Visual Studio プロジェクトに追加するときに、パッケージ マネージャーがここに示すように、バージョンより新しくなっている SignalR スクリプト ファイルのバージョンをインストールできます。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Save All**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>このサンプルを実行します。

1. F5 キーを押して、プロジェクトをデバッグ モードで実行します。
2. ブラウザーのアドレスの行で追加**ホーム/チャット**プロジェクトの既定のページの URL にします。 ブラウザーのインスタンスと、ユーザー名のプロンプトで、チャット ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. ユーザー名を入力します。
4. ブラウザーのアドレスの行から URL をコピーし、2 つ以上のブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
5. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**です。 コメントは、すべてのブラウザー インスタンスで表示されます。

    > [!NOTE]
    > この単純なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーにコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加時から追加されたメッセージに表示されます。
6. 次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示します。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**の実行中のアプリケーション ノード。 お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。 という名前のスクリプト ファイルがある**ハブ**SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。 Internet Explorer ではないブラウザーを使用する場合は、アクセスすることも、動的**ハブ**を参照して、直接、たとえば http://mywebsite/signalr/hubs ファイル。

<a id="code"></a>

## <a name="examine-the-code"></a>コードを調べます

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示しています。 サーバーで、メインの調整オブジェクトとしてハブを作成すると、SignalR jQuery ライブラリを使用してメッセージを送信および受信します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**から派生したクラス、 **Microsoft.AspNet.SignalR.Hub**クラスです。 派生する、**ハブ**クラスは、SignalR アプリケーションをビルドする便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページのスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**です。

**送信**メソッドは、ハブの概念をいくつかを示します。

- ハブのパブリック メソッドを宣言のクライアントが呼び出すことができます。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**のすべてのクライアントにアクセスするプロパティがこのハブに接続します。
- クライアントで、関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

**Chat.cshtml**コード サンプルではファイルの表示は、SignalR jQuery ライブラリを使用して SignalR ハブと通信する方法を示しています。 コード内の必須のタスクには、プロキシへの参照、自動生成されたハブのサーバーがクライアントにプッシュ コンテンツを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信が作成されます。

次のコードでは、ハブ プロキシへの参照を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript では、サーバー クラスとそのメンバーへの参照は、camel 形式でです。 コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**です。 参照する場合、`ChatHub`クラス従来 pascal 形式での jQuery の場合と、C# の場合は、大文字小文字の区別、ChatHub.cs クラス ファイルを編集します。 追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。 追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。 参照を jQuery を最後に、更新、`ChatHub`クラスです。


次のコードでは、スクリプトにコールバック関数を作成する方法を示します。 サーバーの hub クラスは、各クライアントにコンテンツの更新プログラムをプッシュするには、この関数を呼び出します。 省略可能な呼び出し、`htmlEncode`関数の表示方法を HTML には、スクリプト インジェクションを防止する方法として、ページに表示する前に、メッセージの内容をエンコードします。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードの接続を開始しますおよびの click イベントを処理する関数を渡します、**送信**チャット ページでボタンをクリックします。

> [!NOTE]
> これにより、イベント ハンドラーが実行される前に、接続が確立されています。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR には、リアルタイムの web アプリケーションを構築するためのフレームワークを学習しました。 いくつかの SignalR 開発タスクについても学びました。 SignalR を ASP.NET アプリケーションに追加する方法、ハブ クラスを作成する方法、およびハブからメッセージを送受信する方法です。

サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。 Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/)です。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
