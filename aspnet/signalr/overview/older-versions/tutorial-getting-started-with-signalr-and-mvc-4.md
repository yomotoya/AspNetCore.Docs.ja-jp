---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'チュートリアル: SignalR の概要 1.x と MVC 4 |Microsoft ドキュメント'
author: pfletcher
description: ASP.NET SignalR と ASP.NET MVC 4 を使用して、リアルタイムのチャット アプリケーションをビルドします。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>チュートリアル: SignalR の概要 1.x と MVC 4
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

> このチュートリアルでは、ASP.NET SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR の MVC 4 アプリケーションに追加し、送信、メッセージを表示するチャット ビューを作成します。


## <a name="overview"></a>概要

このチュートリアルでは、ASP.NET SignalR と ASP.NET MVC 4 のリアルタイムの web アプリケーションの開発に紹介します。 チュートリアル、チャット アプリケーションと同じコードを使用して、[チュートリアル SignalR が入門](tutorial-getting-started-with-signalr.md)、インターネット テンプレートに基づいて、MVC 4 アプリケーションに追加する方法を示していますが、します。

このトピックでは、次の SignalR 開発タスクを学習します。

- SignalR ライブラリを MVC 4 アプリケーションに追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成しています。
- Web ページでは、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブからの更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されている完了したチャット アプリケーションを示します。

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [このサンプルを実行します。](#run)
- [コードを調べます](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

必要条件:

- Visual Studio 2010 SP1、Visual Studio 2012、または Visual Studio 2012 Express です。 Visual Studio がない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)空き Visual Studio 2012 Express 開発ツールを取得します。
- Visual Studio 2010 をインストールする[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)です。

このセクションでは、ASP.NET MVC 4 アプリケーションを作成し、SignalR ライブラリを追加し、チャット アプリケーションを作成する方法を示します。

1. 1. Visual Studio で ASP.NET MVC 4 アプリケーションを作成し、SignalRChat、という名前を [ok] をクリックします。

        > [!NOTE]
        > VS 2010 で選択**.NET Framework 4**のフレームワークのバージョンのドロップダウン コントロールでします。 SignalR のコードは、.NET Framework version 4 および 4.5 で実行されます。

        ![Mvc web を作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. インターネット アプリケーション テンプレートを選択し、オプションをオフに**単体テスト プロジェクトを作成**、[ok] をクリックします。

         ![Mvc のインターネット サイトを作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. 開く、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**し、次のコマンドを実行します。 この手順では、一連のスクリプト ファイルと SignalR 機能を有効にするアセンブリ参照をプロジェクトに追加します。

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. **ソリューション エクスプ ローラー** Scripts フォルダーを展開します。 SignalR のスクリプト ライブラリがプロジェクトに追加されたことに注意してください。

         ![ライブラリの参照](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**ハブ**です。
      6. 右クリックし、**ハブ**フォルダーで、をクリックして**追加 |クラス**、新しい c# という名前のクラスを作成および**ChatHub.cs**です。 このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。

> [!NOTE]
> Visual Studio 2012 を使用してインストールした場合、 [ASP.NET および Web ツール 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)、ハブ クラスを作成する新しい SignalR 項目テンプレートを使用することができます。 右クリックし、**ハブ**フォルダーで、をクリックして**追加 |新しい項目の** **SignalR ハブ クラス (v1)**、クラスの名前と**ChatHub.cs**です。


1. コードで置き換え、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. 開く、 **Global.asax** 、プロジェクトのファイルし、メソッドの呼び出しを追加`RouteTable.Routes.MapHubs();`内のコードの最初の行として、`Application_Start`メソッドです。 このコードは、SignalR ハブの既定のルートを登録し、他のルートを登録する前に呼び出す必要があります。 完成した`Application_Start`メソッドは次の例のようになります。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. 編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加するとします。 このメソッドが戻る、**チャット**後の手順で作成するビュー。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. 内で右クリックし、`Chat`メソッドを作成してをクリックして**ビューの追加**を新しいファイルの表示を作成します。
5. **ビューの追加**ダイアログ ボックスで、チェック ボックスが選択されていることを確認してください**レイアウトまたはマスター ページを使用して**(その他のチェック ボックスをオフ)、をクリックして**追加**です。

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. という新しいビュー ファイルを編集**Chat.cshtml**です。 後に、 &lt;h2&gt;タグで、以下を貼り付けます&lt;div&gt;セクションと`@section scripts`コード ブロックをページにします。 このスクリプトは、チャット メッセージを送信し、サーバーからメッセージを表示するページを有効にします。 次のコード ブロックにチャット ビューの完全なコードが表示されます。

    > [!IMPORTANT]
    > SignalR と他のスクリプト ライブラリを Visual Studio プロジェクトに追加するときにパッケージ マネージャーはこのトピックに示すバージョンより新しくなっているスクリプトのバージョンをインストール可能性があります。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンを一致することを確認してください。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Save All**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>このサンプルを実行します。

1. F5 キーを押して、プロジェクトをデバッグ モードで実行します。
2. ブラウザーのアドレスの行で追加**ホーム/チャット**プロジェクトの既定のページの URL にします。 ブラウザーのインスタンスと、ユーザー名のプロンプトで、チャット ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. ユーザー名を入力します。
4. ブラウザーのアドレスの行から URL をコピーし、2 つ以上のブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
5. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**です。 コメントは、すべてのブラウザー インスタンスで表示されます。

    > [!NOTE]
    > この単純なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーにコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加時から追加されたメッセージに表示されます。
6. 次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示します。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**の実行中のアプリケーション ノード。 お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。 という名前のスクリプト ファイルがある**ハブ**SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。 Internet Explorer ではないブラウザーを使用する場合は、アクセスすることも、動的**ハブ**ファイルを参照して、直接、たとえばhttp://mywebsite/signalr/hubsします。

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを調べます

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示しています。 サーバーで、メインの調整オブジェクトとしてハブを作成すると、SignalR jQuery ライブラリを使用してメッセージを送信および受信します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**から派生したクラス、 **Microsoft.AspNet.SignalR.Hub**クラスです。 派生する、**ハブ**クラスは、SignalR アプリケーションをビルドする便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページの jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**です。

**送信**メソッドは、ハブの概念をいくつかを示します。

- ハブのパブリック メソッドを宣言のクライアントが呼び出すことができます。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**のすべてのクライアントにアクセスするプロパティがこのハブに接続します。
- クライアントで jQuery 関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

**Chat.cshtml**コード サンプルではファイルの表示は、SignalR jQuery ライブラリを使用して SignalR ハブと通信する方法を示しています。 コード内の必須のタスクには、プロキシへの参照、自動生成されたハブのサーバーがクライアントにプッシュ コンテンツを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信が作成されます。

次のコードでは、ハブのプロキシを宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> JQuery では、サーバー クラスとそのメンバーへの参照は、camel 形式でです。 コード サンプルを参照して、c# **ChatHub**クラスとして jQuery の**chatHub**です。 参照する場合、`ChatHub`クラス従来 pascal 形式での jQuery の場合と、C# の場合は、大文字小文字の区別、ChatHub.cs クラス ファイルを編集します。 追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。 追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。 参照を jQuery を最後に、更新、`ChatHub`クラスです。


次のコードでは、スクリプトにコールバック関数を作成する方法を示します。 サーバーの hub クラスは、各クライアントにコンテンツの更新プログラムをプッシュするには、この関数を呼び出します。 省略可能な呼び出し、`htmlEncode`関数の表示方法を HTML には、スクリプト インジェクションを防止する方法として、ページに表示する前に、メッセージの内容をエンコードします。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードの接続を開始しますおよびの click イベントを処理する関数を渡します、**送信**チャット ページでボタンをクリックします。

> [!NOTE]
> これにより、イベント ハンドラーが実行される前に、接続が確立されています。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR には、リアルタイムの web アプリケーションを構築するためのフレームワークを学習しました。 いくつかの SignalR 開発タスクについても学びました。 SignalR を ASP.NET アプリケーションに追加する方法、ハブ クラスを作成する方法、およびハブからメッセージを送受信する方法です。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR Project](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
