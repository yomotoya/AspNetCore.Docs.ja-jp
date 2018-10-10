---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'チュートリアル: SignalR の概要 1.x |Microsoft Docs'
author: pfletcher
description: ASP.NET SignalR を使用すると、HTML ページで、リアルタイムのチャット アプリケーションを作成します。
ms.author: riande
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: d541dad19d8fd547d61e8850d64e514ea5db7fcf
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912424"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>チュートリアル: SignalR の概要 1.x
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

> このチュートリアルでは、SignalR を使用してリアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を空の ASP.NET web アプリケーションに追加し、送信し、メッセージを表示する HTML ページを作成します。


## <a name="overview"></a>概要

このチュートリアルでは、ブラウザー ベースの単純なチャット アプリケーションを構築する方法を示す、SignalR の開発について説明します。 SignalR ライブラリ空の ASP.NET web アプリケーションを追加、クライアントにメッセージを送信するためのハブ クラスを作成、およびユーザー チャット メッセージを送受信できるようにする HTML ページが作成されます。 MVC 4 のチャット アプリケーションを作成する方法を示しています、MVC ビューを使用して類似のチュートリアルを参照してください。 [SignalR と MVC 4 の概要](index.md)します。

> [!NOTE]
> このチュートリアルでは、SignalR のリリース (1.x) バージョンを使用します。 SignalR の間の変更の詳細についての 1.x と 2.0 を参照してください。[アップグレード SignalR 1.x プロジェクト](../releases/upgrading-signalr-1x-projects-to-20.md)します。

SignalR はライブ ユーザーとの対話やリアルタイムのデータの更新プログラムを必要とする web アプリケーションを構築するためのオープン ソース .NET ライブラリです。 例には、ソーシャル アプリケーション、ゲームのマルチ ユーザー、ビジネス コラボレーション、およびニュース、天気、またはアプリケーションの財務の更新が含まれます。 これらは多くの場合、リアルタイム アプリケーションと呼ばれます。

SignalR は、リアルタイム アプリケーションの構築プロセスを簡略化します。 これには、ASP.NET のサーバー ライブラリとクライアント サーバー接続を管理し、クライアントにコンテンツの更新をプッシュするが簡単に JavaScript クライアント ライブラリが含まれます。 SignalR ライブラリは、リアルタイムの機能を利用する既存の ASP.NET アプリケーションを追加できます。

このチュートリアルでは、次の SignalR 開発タスクを示しています。

- ASP.NET web アプリケーションに SignalR ライブラリを追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成します。
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

SignalR を追加するこのセクションでは、空の ASP.NET web アプリケーションを作成する方法を示していて、チャット アプリケーションを作成します。

必要条件:

- Visual Studio 2010 SP1 または 2012。 Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2012 Express 開発ツールを取得します。
- [Microsoft ASP.NET および Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)します。 Visual Studio 2012 の場合は、このインストーラーは、Visual Studio に SignalR テンプレートを含む ASP.NET の新機能を追加します。 Visual Studio 2010 SP1 では、インストーラーは使用できませんが、セットアップ手順」の説明に従って、SignalR の NuGet パッケージをインストールすることで、チュートリアルを行うことができます。

次の手順では、空の ASP.NET Web アプリケーションを作成し、SignalR ライブラリを追加する Visual Studio 2012 を使用します。

1. Visual Studio では、空の ASP.NET Web アプリケーションを作成します。

    ![空の web を作成します。](tutorial-getting-started-with-signalr/_static/image2.png)
2. 開く、**パッケージ マネージャー コンソール**を選択して**ツール |NuGet パッケージ マネージャー |パッケージ マネージャー コンソール**します。 コンソール ウィンドウには、次のコマンドを入力します。

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    このコマンドは、SignalR の最新バージョンをインストールします。 1.x します。
3. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |クラス**します。 新しいクラスの名前**ChatHub**します。
4. **ソリューション エクスプ ローラー**スクリプト ノードを展開します。 JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。

    ![ライブラリの参照](tutorial-getting-started-with-signalr/_static/image3.png)
5. コードに置き換えます、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、**グローバル アプリケーション クラス**クリック**追加**します。

    ![グローバルの追加します。](tutorial-getting-started-with-signalr/_static/image4.png)
7. 次の追加`using`後にある、指定されたステートメント`using`Global.asax.cs クラス内のステートメント。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 次のコードの行を追加、 `Application_Start` SignalR ハブの既定のルートを登録するグローバル クラスのメソッド。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、クリックして**追加 |新しい項目の**します。 **新しい項目の追加**ダイアログ、選択の Html ページとクリック**追加**します。
10. **ソリューション エクスプ ローラー**で作成した HTML ページを右クリックし、をクリックして**スタート ページとして設定**します。
11. HTML ページの既定のコードを次のコードに置き換えます。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **すべて保存**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>サンプルを実行します。

1. F5 キーを押して、デバッグ モードで、プロジェクトを実行します。 ブラウザーのインスタンスとユーザー名のプロンプトで、HTML ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr/_static/image5.png)
2. ユーザー名を入力します。
3. ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
4. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。 すべてのブラウザー インスタンスで、コメントが表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。

    次のスクリーン ショットは、1 つのインスタンス メッセージを送信するすべての更新は、次の 3 つのブラウザー インスタンスで実行されるチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr/_static/image6.png)
5. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを確認します

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs"></a>SignalR ハブ

コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。 派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページに jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.broadcastMessage**します。

**送信**メソッドがいくつかのハブの概念を示します。

- クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスする動的プロパティ。
- クライアントの jQuery 関数を呼び出す (など、`broadcastMessage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

コード サンプルでは、HTML ページは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。 コードで基本的なタスクは、プロキシをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、ハブにメッセージを送信する接続を開始、ハブの参照を宣言することです。

次のコードは、ハブのプロキシを宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> JQuery でサーバー クラスとそのメンバーへの参照がキャメル ケースです。 コード サンプルを参照して、c# **ChatHub**クラスとしての jquery **chatHub**します。


次のコードは、スクリプトのコールバック関数を作成する方法です。 サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 HTML が表示する前にコンテンツをエンコードする 2 つの行は省略可能ですし、スクリプト インジェクションを防止する簡単な方法を表示します。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**HTML ページにボタンをクリックします。

> [!NOTE]
> このアプローチでは、イベント ハンドラーが実行される前に、接続が確立されていることを保証します。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。 いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。

利用できるサンプル アプリケーションは、このチュートリアルやその他の SignalR アプリケーションで、インターネット経由でしてホスティング プロバイダーにデプロイすること。 Microsoft 提供の無料 web ホスティングを無料で最大 10 個の web サイトを[Windows Azure トライアル アカウント](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。 サンプルの SignalR アプリケーションをデプロイする方法のチュートリアルは、次を参照してください。 [、SignalR 入門サンプルとして Windows Azure の Web サイトを発行](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)します。 Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [ASP.NET アプリケーションを Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)します。 (注: WebSocket トランスポートでは現在の Windows Azure Web サイトがサポートされていません。 ときに WebSocket トランスポートを使用できません、SignalR のトランスポートのセクションで説明したように、その他の利用可能なトランスポートを使用して、 [SignalR のトピックで初めに](index.md))。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)
