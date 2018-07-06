---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR 2 の概要 |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加され、HTML pa を作成しています.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 798838af099cceb12652b7c6c66633a03a73e538
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841843"
---
<a name="tutorial-getting-started-with-signalr-2"></a>チュートリアル: SignalR 2 の概要
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。 
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

このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発について説明します。 SignalR ライブラリ空の ASP.NET web アプリケーションを追加、クライアントにメッセージを送信するためのハブ クラスを作成、およびユーザー チャット メッセージを送受信できるようにする HTML ページが作成されます。 MVC 5 でのチャット アプリケーションを作成する方法を示しています、MVC ビューを使用して類似のチュートリアルを参照してください。 [SignalR 2 と MVC 5 の概要](tutorial-getting-started-with-signalr-and-mvc.md)します。

> [!NOTE]
> このチュートリアルでは、バージョン 2 で SignalR アプリケーションを作成する方法を示します。 SignalR の間の変更の詳細についての 1.x と 2 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)と[Visual Studio 2013 リリース ノート](../../../visual-studio/overview/2013/release-notes.md#TOC13)します。

SignalR はライブ ユーザーとの対話やリアルタイムのデータの更新プログラムを必要とする web アプリケーションを構築するためのオープン ソース .NET ライブラリです。 例には、ソーシャル アプリケーション、ゲームのマルチ ユーザー、ビジネス コラボレーション、およびニュース、天気、またはアプリケーションの財務の更新が含まれます。 これらは多くの場合、リアルタイム アプリケーションと呼ばれます。

SignalR は、リアルタイム アプリケーションの構築プロセスを簡略化します。 これには、ASP.NET のサーバー ライブラリとクライアント サーバー接続を管理し、クライアントにコンテンツの更新をプッシュするが簡単に JavaScript クライアント ライブラリが含まれます。 SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションを追加できます。

このチュートリアルでは、次の SignalR 開発タスクを示しています。

- ASP.NET web アプリケーションに SignalR ライブラリを追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成します。
- アプリケーションを構成する OWIN startup クラスを作成します。
- Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。 各新規ユーザー コメントを投稿でき、ユーザーがチャットに参加した後に追加されたコメントを参照してください。

![チャット インスタンス](tutorial-getting-started-with-signalr/_static/image1.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [サンプルを実行します。](#run)
- [コードを確認します](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

SignalR を追加するこのセクションでは、Visual Studio 2013 と SignalR バージョン 2 を使用して、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。

必要条件:

- Visual Studio 2013. Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2013 Express 開発ツールを取得します。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2013 を使用します。

1. Visual Studio で ASP.NET Web アプリケーションを作成します。

    ![Web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. **新しい ASP.NET プロジェクト** ウィンドウのままに**空**選択およびクリックして**プロジェクトの作成**。

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image3.png)
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |SignalR ハブ クラス (v2)** します。 クラスの名前**ChatHub.cs**し、プロジェクトに追加します。 この手順で作成、 **ChatHub**クラスし、一連のスクリプト ファイルと SignalR をサポートするアセンブリ参照をプロジェクトに追加します。

    > [!NOTE]
    > 開き、プロジェクトに SignalR を追加することも、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**コマンドを実行しているとします。

    `install-package Microsoft.AspNet.SignalR`

    SignalR を追加する、コンソールを使用する場合は、SignalR を追加した後に、別のステップとして、SignalR ハブ クラスを作成します。

    > [!NOTE]
    > Visual Studio 2012 を使用している場合、 **SignalR ハブ クラス (v2)** テンプレートは使用できません。 プレーンなを追加する**クラス**と呼ばれる`ChatHub`代わりにします。
4. **ソリューション エクスプ ローラー**スクリプトを展開します。 JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。
5. 新しいコードを置き換える**ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |OWIN Startup クラス**します。 新しいクラスの名前`Startup`[ok] をクリックします。

    > [!NOTE]
    > Visual Studio 2012 を使用している場合、 **OWIN Startup クラス**テンプレートは使用できません。 プレーンなを追加する**クラス**と呼ばれる`Startup`代わりにします。
7. 次に、新しいスタートアップ クラスの内容を変更します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |HTML ページ**します。 名前を新しいページ`index.html`します。
    >[!NOTE]
    >JQuery と SignalR ライブラリへの参照のバージョン番号を変更する必要があります。
9. **ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、をクリックして**スタート ページとして設定**します。
10. HTML ページの既定のコードを次のコードに置き換えます。

    > [!NOTE]
    > 以降のバージョンの SignalR スクリプトは、パッケージ マネージャーによってインストールされます。 次のスクリプト参照が (される SignalR ハブを追加するのではなく、NuGet を使用して追加した場合は、異なるします。) プロジェクトのスクリプト ファイルのバージョンに対応することを確認します。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **すべて保存**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>サンプルを実行します。

1. F5 キーを押して、デバッグ モードで、プロジェクトを実行します。 ブラウザーのインスタンスとユーザー名のプロンプトで、HTML ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image4.png)
2. ユーザー名を入力します。
3. ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
4. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。 すべてのブラウザー インスタンスで、コメントが表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。

    次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されるチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image5.png)
5. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを確認します

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。 派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**します。

**送信**メソッドがいくつかのハブの概念を示します。

- クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスする動的プロパティ。
- クライアントで関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。 コードで基本的なタスクは、プロキシをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。

次のコードでは、ハブ プロキシへの参照を宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript でサーバー クラスとそのメンバーへの参照がキャメル ケースです。 コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**します。


次のコードは、スクリプトのコールバック関数を作成する方法です。 サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 HTML が表示する前にコンテンツをエンコードする 2 つの行は省略可能ですし、スクリプト インジェクションを防止する簡単な方法を表示します。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。

> [!NOTE]
> このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。 いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。

サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)
