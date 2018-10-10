---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'チュートリアル: SignalR セルフホスト |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、SignalR 2 の自己ホスト型サーバーを作成する方法と、JavaScript クライアントで接続する方法を説明します。 ソフトウェアのバージョンが V のチュートリアルで使用しています.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: a08ce2e89ae13125cbc3915b44bcd1120fc22150
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911533"
---
<a name="tutorial-signalr-self-host"></a>チュートリアル: SignalR セルフホスト
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> このチュートリアルでは、SignalR 2 の自己ホスト型サーバーを作成する方法と、JavaScript クライアントで接続する方法を説明します。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


## <a name="overview"></a>概要

SignalR のサーバーは通常、IIS で ASP.NET アプリケーションでホストされているが、自己ホスト型にも (コンソール アプリケーションまたは Windows サービスなど)、自己ホスト ライブラリを使用します。 SignalR 2 のすべてと同様に、このライブラリは OWIN 上に構築 ([Open Web Interface for .NET](http://owin.org))。 OWIN は、.NET の web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションを分離します。

IIS でホストしていない理由は次のとおりです。

- IIS が利用できないか、IIS なしの既存のサーバー ファームなどの望ましい環境。
- IIS のパフォーマンスのオーバーヘッドを回避する必要があります。
- SignalR の機能では、Windows サービス、Azure worker ロール、またはその他のプロセスで実行されている既存のアプリケーションに追加します。

ソリューションは、パフォーマンス上の理由から自己ホストとして開発されているが場合、ことをお勧めもテストを IIS でホストされているアプリケーションでパフォーマンスの向上を特定します。

このチュートリアルには、次のセクションが含まれています。

- [サーバーの作成](#server)
- [JavaScript クライアントとサーバーへのアクセス](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>サーバーの作成

このチュートリアルでは、コンソール アプリケーションでホストされているサーバーを作成しますが、サーバーは、あらゆる種類の Windows サービスまたは Azure ワーカー ロールなどのプロセスでホストできます。 Windows サービスで SignalR のサーバーをホストするためのサンプル コードを参照してください。 [Windows サービスで Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)します。

1. 管理者特権で Visual Studio 2013 を開きます。 選択**ファイル**、**新しいプロジェクト**します。 選択**Windows**下、 **Visual c#** 内のノード、**テンプレート**ペイン、および選択、**コンソール アプリケーション**テンプレート。 新しいプロジェクトの名前を"SignalRSelfHost"にして**OK**します。

    ![](tutorial-signalr-self-host/_static/image1.png)
2. 選択して、NuGet パッケージ マネージャー コンソールを開きます**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。
3. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    このコマンドは、プロジェクトに SignalR 2 Self-Host ライブラリを追加します。
4. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    このコマンドは、Microsoft.Owin.Cors ライブラリをプロジェクトに追加します。 このライブラリは、SignalR と web ページのクライアントに別のドメインをホストしているアプリケーションに必要なクロス ドメインのサポートに使用されます。 つまり、SignalR サーバーと異なるポートを web クライアントがホストするとします、ために、クロス ドメインは、これらのコンポーネント間の通信を有効にする必要があります。
5. Program.cs の内容を次のコードで置き換えます。

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    上記のコードには、3 つのクラスが含まれています。

    - **プログラム**など、 **Main**メソッドの実行のプライマリ パスを定義します。 このメソッドでは、型の web アプリケーションで**スタートアップ**が開始されて、指定した URL (`http://localhost:8080`)。 エンドポイントのセキュリティが必要な場合は、SSL を実装できます。 参照してください[方法: SSL 証明書でポートを構成](https://msdn.microsoft.com/library/ms733791.aspx)詳細についてはします。
    - **スタートアップ**、SignalR のサーバーの構成を含むクラス (このチュートリアルでは、のみの構成への呼び出しは、 `UseCors`) への呼び出し`MapSignalR`ハブのすべてのオブジェクトのルート プロジェクトを作成します。
    - **MyHub**アプリケーションをクライアントに提供する SignalR ハブ クラス。 このクラスは 1 つのメソッド、**送信**、接続されている他のすべてのクライアントにメッセージをブロードキャストするクライアントが呼び出すことです。
6. アプリケーションをコンパイルして実行します。 サーバーを実行しているアドレスは、コンソール ウィンドウに表示されます。

    ![](tutorial-signalr-self-host/_static/image2.png)
7. 例外によって実行が失敗した場合`System.Reflection.TargetInvocationException was unhandled`、管理者特権で Visual Studio を再起動する必要があります。
8. 次のセクションに進む前にアプリケーションを停止します。

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>JavaScript クライアントとサーバーへのアクセス

このセクションでは、同じ JavaScript クライアントから使用します、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)します。 これは、ハブの URL を明示的に定義すると、クライアントに 1 つの変更のみしましょう。 自己ホスト型アプリケーションでは、サーバー必ずしも URL (リバース プロキシとロード バランサー) のための接続と同じアドレスでこれを明示的に定義の URL は。

1. **ソリューション エクスプ ローラー**ソリューションを右クリックし、選択、**追加**、**新しいプロジェクト**します。 選択、 **Web**ノード、および選択、 **ASP.NET Web アプリケーション**テンプレート。 プロジェクトに"JavascriptClient"という名前にして**OK**します。

    ![](tutorial-signalr-self-host/_static/image3.png)
2. 選択、**空**テンプレート、および残りのオプションがオフのままにします。 選択**プロジェクトを作成する**します。

    ![](tutorial-signalr-self-host/_static/image4.png)
3. パッケージ マネージャー コンソールで"JavascriptClient"プロジェクトを選択、**既定のプロジェクト**ドロップダウンで、次のコマンドを実行します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    このコマンドは、クライアントにする必要があります、SignalR と JQuery ライブラリをインストールします。
4. クリックし、プロジェクトを右クリックして**追加**、**新しい項目の**します。 選択、 **Web**ノード、および HTML ページを選択します。 ページの名前を付けます**Default.html**します。

    ![](tutorial-signalr-self-host/_static/image5.png)
5. 新しい HTML ページの内容を次のコードに置き換えます。 ここで、スクリプト参照がプロジェクトの Scripts フォルダー内のスクリプトと一致することを確認します。

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    次のコード (上記のコード例で強調表示) とは、(SignalR バージョン 2 の beta へのコードのアップグレード) に加えて、よろめき取得のチュートリアルで使用するクライアントに適用した追加です。 次のコードでは SignalR の基本的な接続 URL が明示的にサーバー上に設定します。

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. ソリューションを右クリックして**スタートアップ プロジェクトの設定.**.選択、**マルチ スタートアップ プロジェクト**ボタンをクリックし、両方のプロジェクトの設定**アクション**に**開始**します。

    ![](tutorial-signalr-self-host/_static/image6.png)
7. "Default.html"を右クリックし、選択**スタート ページとして設定**します。
8. アプリケーションを実行します。 サーバーおよびページが起動します。 Web ページを再読み込みする必要があります (または選択**続行**デバッガーで) サーバーを開始する前に、ページが読み込まれる場合。
9. ブラウザーでは、入力を求められたら、ユーザー名を提供します。 別のブラウザー タブまたはウィンドウに、ページの URL をコピーして、別のユーザー名を指定します。 チュートリアル入門のように、他の 1 つのブラウザー ウィンドウからメッセージを送信することができます。
