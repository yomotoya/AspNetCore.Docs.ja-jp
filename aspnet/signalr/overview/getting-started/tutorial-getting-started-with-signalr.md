---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR 2 の概要 |Microsoft ドキュメント'
author: pfletcher
description: このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、HTML pa を作成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036805"
---
<a name="tutorial-getting-started-with-signalr-2"></a>チュートリアル: SignalR 2 の概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、送信、メッセージを表示する HTML ページを作成します。 
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

このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発を紹介します。 SignalR ライブラリを空の ASP.NET web アプリケーションに追加は、クライアントにメッセージを送信するため、ハブ クラスを作成し、ユーザーがチャット メッセージを送信できる HTML ページを作成します。 MVC ビューを使用する MVC 5 でチャット アプリケーションを作成する方法を説明する類似のチュートリアルを参照してください。 [SignalR 2 と MVC 5 の概要](tutorial-getting-started-with-signalr-and-mvc.md)です。

> [!NOTE]
> このチュートリアルでは、バージョン 2 で SignalR アプリケーションを作成する方法について説明します。 SignalR の間の変更の詳細 1.x と 2 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)と[Visual Studio 2013 のリリース ノート](../../../visual-studio/overview/2013/release-notes.md#TOC13)です。

SignalR は、ライブのユーザーの介入またはリアルタイムのデータ更新を必要とする web アプリケーションを構築するため、オープン ソースの .NET ライブラリです。 例には、ソーシャル アプリケーション、マルチ ユーザー ゲーム、ビジネスのコラボレーション、およびニュース、天気、またはアプリケーションの財務更新が含まれます。 これらは、リアルタイム アプリケーションとも呼ばれます。

SignalR には、リアルタイム アプリケーションを構築するプロセスが簡略化します。 ASP.NET サーバー ライブラリとクライアント/サーバー接続の管理およびコンテンツの更新プログラムをクライアント プッシュを容易にできるように、JavaScript クライアント ライブラリが含まれています。 SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションに追加できます。

このチュートリアルでは、次の SignalR 開発タスクを示しています。

- ASP.NET web アプリケーションには、SignalR ライブラリを追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成しています。
- アプリケーションを構成する OWIN スタートアップ クラスを作成します。
- Web ページでは、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブからの更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示します。 それぞれの新しいユーザーは、コメントを投稿し、ユーザー、チャットに参加した後に追加されたコメントを確認できます。

![チャット インスタンス](tutorial-getting-started-with-signalr/_static/image1.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [このサンプルを実行します。](#run)
- [コードを調べます](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

SignalR では、このセクションでは、Visual Studio 2013、SignalR バージョン 2 を使用して、空の ASP.NET web アプリケーションを作成する方法を示しています。 追加し、チャット アプリケーションを作成します。

必要条件:

- Visual Studio 2013. Visual Studio がない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)空き Visual Studio 2013 Express 開発ツールを取得します。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2013 を使用します。

1. Visual Studio では、ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. **新しい ASP.NET プロジェクト** ウィンドウのままにして**空**を選択し、をクリックして**プロジェクトの作成**です。

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image3.png)
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)** です。 クラスの名前を付けます**ChatHub.cs**し、プロジェクトに追加します。 この手順で作成、 **ChatHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照がプロジェクトに追加します。

    > [!NOTE]
    > 開き、プロジェクトに SignalR を追加することも、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。

    `install-package Microsoft.AspNet.SignalR`

    コンソールを使用して SignalR を追加する場合は、SignalR を追加した後に、別のステップとして SignalR ハブ クラスを作成します。

    > [!NOTE]
    > Visual Studio 2012 を使用している場合、 **SignalR ハブ クラス (v2)** テンプレートは使用できません。 形式を追加する**クラス**と呼ばれる`ChatHub`代わりにします。
4. **ソリューション エクスプ ローラー**、[スクリプト] ノードを展開します。 JQuery、および SignalR のスクリプト ライブラリは、プロジェクトに表示されます。
5. 新しいコードを置き換える**ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN 起動クラス**です。 新しいクラスの名前を`Startup`OK をクリックします。

    > [!NOTE]
    > Visual Studio 2012 を使用している場合、 **OWIN スタートアップ クラス**テンプレートは使用できません。 形式を追加する**クラス**と呼ばれる`Startup`代わりにします。
7. 新しいスタートアップ クラスの内容を次のように変更します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |HTML ページ**です。 新しいページの名前を付けます`index.html`です。
    >[!NOTE]
    >JQuery、および SignalR のライブラリへの参照のバージョン番号を変更する必要があります。
9. **ソリューション エクスプ ローラー**を作成する HTML ページを右クリックし、をクリックして**スタート ページとして設定**です。
10. HTML ページで、既定のコードを次のコードに置き換えます。

    > [!NOTE]
    > 以降のバージョンの SignalR スクリプトは、パッケージ マネージャーでインストールする可能性があります。 次のスクリプト参照を (それらされます異なる SignalR ハブを追加するのではなく、NuGet を使用して追加した場合)、プロジェクト内のスクリプト ファイルのバージョンに対応することを確認してください。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Save All**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>このサンプルを実行します。

1. F5 キーを押して、プロジェクトをデバッグ モードで実行します。 ブラウザーのインスタンスと、ユーザー名のプロンプトで、HTML ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image4.png)
2. ユーザー名を入力します。
3. ブラウザーのアドレスの行から URL をコピーし、2 つ以上のブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
4. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**です。 コメントは、すべてのブラウザー インスタンスで表示されます。

    > [!NOTE]
    > この単純なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーにコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加時から追加されたメッセージに表示されます。

    次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されているチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image5.png)
5. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**の実行中のアプリケーション ノード。 という名前のスクリプト ファイルがある**ハブ**SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを調べます

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示しています。 サーバーで、メインの調整オブジェクトとしてハブを作成すると、SignalR jQuery ライブラリを使用してメッセージを送信および受信します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**から派生したクラス、 **Microsoft.AspNet.SignalR.Hub**クラスです。 派生する、**ハブ**クラスは、SignalR アプリケーションをビルドする便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページのスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**です。

**送信**メソッドは、ハブの概念をいくつかを示します。

- ハブのパブリック メソッドを宣言のクライアントが呼び出すことができます。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**のすべてのクライアントにアクセスする動的なプロパティがこのハブに接続します。
- クライアントで、関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して SignalR ハブと通信する方法を示します。 コード内の必須タスクは、プロキシ サーバーがクライアントにプッシュ コンテンツを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。

次のコードでは、ハブ プロキシへの参照を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript では、サーバー クラスとそのメンバーへの参照は、camel 形式でです。 コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**です。


次のコードは、スクリプトにコールバック関数を作成する方法です。 サーバーの hub クラスは、各クライアントにコンテンツの更新プログラムをプッシュするには、この関数を呼び出します。 HTML が表示する前に、コンテンツをエンコードする 2 つの行は、省略可能なスクリプト インジェクションを防止する簡単な方法を示します。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードの接続を開始しますおよびの click イベントを処理する関数を渡します、**送信**HTML ページにボタンをクリックします。

> [!NOTE]
> これにより、イベント ハンドラーが実行される前に、接続が確立されています。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR には、リアルタイムの web アプリケーションを構築するためのフレームワークを学習しました。 いくつかの SignalR 開発タスクについても学びました。 SignalR を ASP.NET アプリケーションに追加する方法、ハブ クラスを作成する方法、およびハブからメッセージを送受信する方法です。

サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルについては、次を参照してください。 [Azure App service Web アプリを使用してを使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)です。 Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR Project](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
