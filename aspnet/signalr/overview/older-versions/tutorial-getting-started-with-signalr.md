---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "チュートリアル: SignalR の概要 1.x |Microsoft ドキュメント"
author: pfletcher
description: "HTML ページにリアルタイムのチャット アプリケーションをビルドするのにには、ASP.NET SignalR を使用します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c61be6f7a64c000c8d9489f35eea520fd0bb32dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x"></a>チュートリアル: SignalR の概要 1.x
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

> このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、送信、メッセージを表示する HTML ページを作成します。


## <a name="overview"></a>概要

このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発を紹介します。 SignalR ライブラリを空の ASP.NET web アプリケーションに追加は、クライアントにメッセージを送信するため、ハブ クラスを作成し、ユーザーがチャット メッセージを送信できる HTML ページを作成します。 MVC ビューを使用して MVC 4 でチャット アプリケーションを作成する方法を説明する類似のチュートリアルを参照してください。 [SignalR と MVC 4 の概要](index.md)です。

> [!NOTE]
> このチュートリアルでは、SignalR のリリース (1.x) バージョンを使用します。 SignalR の間の変更の詳細 1.x および 2.0 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)です。

SignalR は、ライブのユーザーの介入またはリアルタイムのデータ更新を必要とする web アプリケーションを構築するため、オープン ソースの .NET ライブラリです。 例には、ソーシャル アプリケーション、マルチ ユーザー ゲーム、ビジネスのコラボレーション、およびニュース、天気、またはアプリケーションの財務更新が含まれます。 これらは、リアルタイム アプリケーションとも呼ばれます。

SignalR には、リアルタイム アプリケーションを構築するプロセスが簡略化します。 ASP.NET サーバー ライブラリとクライアント/サーバー接続の管理およびコンテンツの更新プログラムをクライアント プッシュを容易にできるように、JavaScript クライアント ライブラリが含まれています。 SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションに追加できます。

このチュートリアルでは、次の SignalR 開発タスクを示しています。

- ASP.NET web アプリケーションには、SignalR ライブラリを追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成しています。
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

SignalR、このセクションでは、空の ASP.NET web アプリケーションを作成する方法を示しています。 追加し、チャット アプリケーションを作成します。

必要条件:

- Visual Studio 2010 SP1 または 2012 です。 Visual Studio がない場合は、次を参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)空き Visual Studio 2012 Express 開発ツールを取得します。
- [Microsoft ASP.NET および Web ツール 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)です。 Visual Studio 2012 では、このインストーラーは、Visual Studio に SignalR テンプレートを含む ASP.NET の新機能を追加します。 Visual Studio 2010 sp1 では、インストーラーは使用できないセットアップ手順」の説明に従って、SignalR の NuGet パッケージをインストールすることで、チュートリアルを完了することができます。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2012 を使用します。

1. Visual Studio では、空の ASP.NET Web アプリケーションを作成します。

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. 開く、 **Package Manager Console**を選択して**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。 コンソール ウィンドウに次のコマンドを入力します。

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    このコマンドは、SignalR の最新バージョンをインストール 1.x です。
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |クラス**です。 新しいクラスの名前を**ChatHub**です。
4. **ソリューション エクスプ ローラー**スクリプト ノードを展開します。 JQuery、および SignalR のスクリプト ライブラリは、プロジェクトに表示されます。

    ![ライブラリの参照](tutorial-getting-started-with-signalr/_static/image3.png)
5. コードで置き換え、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログで、**グローバル アプリケーション クラス** をクリック**追加**です。

    ![グローバルの追加します。](tutorial-getting-started-with-signalr/_static/image4.png)
7. 次の追加`using`、提供された後のステートメント`using`Global.asax.cs クラス内のステートメント。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 次のコードの行を追加、 `Application_Start` SignalR ハブの既定のルートを登録するグローバル クラスのメソッドです。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログでは、select の Html ページおよび をクリック**追加**です。
10. **ソリューション エクスプ ローラー**を作成する HTML ページを右クリックし、をクリックして**スタート ページとして設定**です。
11. HTML ページで、既定のコードを次のコードに置き換えます。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Save All**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>このサンプルを実行します。

1. F5 キーを押して、プロジェクトをデバッグ モードで実行します。 ブラウザーのインスタンスと、ユーザー名のプロンプトで、HTML ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image5.png)
2. ユーザー名を入力します。
3. ブラウザーのアドレスの行から URL をコピーし、2 つ以上のブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
4. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**です。 コメントは、すべてのブラウザー インスタンスで表示されます。

    > [!NOTE]
    > この単純なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーにコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加時から追加されたメッセージに表示されます。

    次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されているチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image6.png)
5. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**の実行中のアプリケーション ノード。 という名前のスクリプト ファイルがある**ハブ**SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを調べます

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示しています。 サーバーで、メインの調整オブジェクトとしてハブを作成すると、SignalR jQuery ライブラリを使用してメッセージを送信および受信します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**から派生したクラス、 **Microsoft.AspNet.SignalR.Hub**クラスです。 派生する、**ハブ**クラスは、SignalR アプリケーションをビルドする便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページの jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**です。

**送信**メソッドは、ハブの概念をいくつかを示します。

- ハブのパブリック メソッドを宣言のクライアントが呼び出すことができます。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**のすべてのクライアントにアクセスする動的なプロパティがこのハブに接続します。
- クライアントで jQuery 関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して SignalR ハブと通信する方法を示します。 コード内の必須タスクは、プロキシ サーバーがクライアントにプッシュ コンテンツを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。

次のコードでは、ハブのプロキシを宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> JQuery では、サーバー クラスとそのメンバーへの参照は、camel 形式でです。 コード サンプルを参照して、c# **ChatHub**クラスとして jQuery の**chatHub**です。


次のコードは、スクリプトにコールバック関数を作成する方法です。 サーバーの hub クラスは、各クライアントにコンテンツの更新プログラムをプッシュするには、この関数を呼び出します。 HTML が表示する前に、コンテンツをエンコードする 2 つの行は、省略可能なスクリプト インジェクションを防止する簡単な方法を示します。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードの接続を開始しますおよびの click イベントを処理する関数を渡します、**送信**HTML ページにボタンをクリックします。

> [!NOTE]
> この方法により、イベント ハンドラーが実行される前に、接続が確立されていることが保証されます。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR には、リアルタイムの web アプリケーションを構築するためのフレームワークを学習しました。 いくつかの SignalR 開発タスクについても学びました。 SignalR を ASP.NET アプリケーションに追加する方法、ハブ クラスを作成する方法、およびハブからメッセージを送受信する方法です。

利用できるサンプル アプリケーションでこのチュートリアルやその他の SignalR アプリケーション、インターネット経由でホスティング プロバイダーに配置しています。 マイクロソフトは、最大 10 個の web サイトを無料での無料の web ホスティングを提供しています[試用アカウントを Windows Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604)です。 サンプルの SignalR アプリケーションを展開する方法のチュートリアルについては、次を参照してください。 [、SignalR 入門サンプルとして Windows Azure Web サイトを発行](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)です。 Visual Studio web プロジェクトを Windows Azure Web サイトを展開する方法の詳細については、次を参照してください。 [ASP.NET アプリケーションを、Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)です。 (注: WebSocket トランスポートは、Windows Azure の Web サイトを現在サポートされていません。 ときに WebSocket トランスポートを使用できません、SignalR のトランスポートのセクションで説明したように、その他の利用可能なトランスポートを使用して、 [SignalR トピックの概要](index.md))。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースの次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
