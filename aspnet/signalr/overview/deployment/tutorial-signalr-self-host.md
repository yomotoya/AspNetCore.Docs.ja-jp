---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'チュートリアル: 自己ホスト SignalR |Microsoft ドキュメント'
author: pfletcher
description: このチュートリアルでは、自己ホスト型の SignalR 2 サーバーを作成する方法と、JavaScript クライアントでそれに接続する方法を説明します。 ソフトウェアのバージョンが V チュートリアルで使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036779"
---
<a name="tutorial-signalr-self-host"></a>チュートリアル: SignalR の自己ホスト
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> このチュートリアルでは、自己ホスト型の SignalR 2 サーバーを作成する方法と、JavaScript クライアントでそれに接続する方法を説明します。
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
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

SignalR のサーバーは通常、IIS で ASP.NET アプリケーションでホストされているが、自己ホスト型こともできます (このようなコンソール アプリケーションまたは Windows サービスと同様に) 自己ホスト ライブラリを使用します。 SignalR 2 のと同様に、このライブラリは OWIN 上に構築 ([.NET 用 Web インターフェイスを開く](http://owin.org))。 OWIN では、.NET の web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN には、web アプリケーション、サーバーは、OWIN のため、IIS の外部で独自のプロセス内の web アプリケーションを自己ホストするために最適ですから切り離します。

IIS でホストしていない理由は次のとおりです。

- IIS が行われていない IIS せず、既存のサーバー ファームなど、むしろ望ましい場合も使用可能な環境です。
- IIS のパフォーマンスのオーバーヘッドを回避する必要があります。
- SignalR の機能は、Windows サービス、Azure ワーカー ロール、またはその他のプロセスで実行されている既存のアプリケーションに追加するのには。

場合は、ソリューションは、自己ホスト パフォーマンス上の理由として開発中は、ことをお勧めもテストに IIS でホストされているアプリケーション パフォーマンスの利点を決定します。

このチュートリアルでは、次のセクションでは、含まれています。

- [サーバーを作成します。](#server)
- [JavaScript クライアントとサーバーへのアクセス](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>サーバーを作成します。

このチュートリアルでは、コンソール アプリケーションでホストされているサーバーを作成しますが、あらゆる種類の Windows サービスまたは Azure ワーカー ロールなどのプロセスで、サーバーをホストできます。 SignalR のサーバーに Windows サービスをホストするためのサンプル コードを参照してください。 [Windows サービスで Self-Hosting SignalR](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3)です。

1. 管理者特権で Visual Studio 2013 を開きます。 選択**ファイル**、**新しいプロジェクト**です。 選択**Windows**下にある、 **Visual c#** 内のノード、**テンプレート**ペイン、および選択、**コンソール アプリケーション**テンプレート。 新しいプロジェクトの名前を"SignalRSelfHost"し、をクリックして**OK**です。

    ![](tutorial-signalr-self-host/_static/image1.png)
2. 選択して、ライブラリ パッケージ マネージャー コンソールを開きます**ツール**、**ライブラリ パッケージ マネージャー**、 **Package Manager Console**です。
3. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    このコマンドは、SignalR 2 Self-Host ライブラリをプロジェクトに追加します。
4. パッケージ マネージャー コンソールで、次のコマンドを入力します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    このコマンドは、Microsoft.Owin.Cors ライブラリをプロジェクトに追加します。 クロス ドメインのサポートは、SignalR と別のドメインで web ページのクライアントをホストするアプリケーションに必要なは、このライブラリが使用されます。 つまり、SignalR のサーバーと異なるポートを web クライアントがホストするが、ので、クロス ドメインは、これらのコンポーネント間の通信に有効にする必要があります。
5. Program.cs の内容を次のコードで置き換えます。

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    上記のコードには、3 つのクラスが含まれています。

    - **プログラム**など、 **Main**メソッドの実行のプライマリ パスを定義します。 このメソッドでは、型の web アプリケーションで**スタートアップ**が url で指定した開始 (`http://localhost:8080`)。 セキュリティが必要な場合、エンドポイントで、SSL を実装することができます。 参照してください[する方法: SSL 証明書でポートを構成する](https://msdn.microsoft.com/library/ms733791.aspx)詳細についてはします。
    - **スタートアップ**、SignalR のサーバーの構成を含むクラス (このチュートリアルではのみの構成への呼び出しは、 `UseCors`)、および呼び出し`MapSignalR`プロジェクト内のすべてのハブ オブジェクト ルートを作成します。
    - **MyHub**アプリケーションをクライアントに提供する SignalR ハブ クラスです。 このクラスには 1 つのメソッド、**送信**、接続されている他のすべてのクライアントにメッセージをブロードキャストにクライアントが呼び出すことです。
6. アプリケーションをコンパイルして実行します。 コンソール ウィンドウで、サーバーが実行されているアドレスが表示されます。

    ![](tutorial-signalr-self-host/_static/image2.png)
7. 例外によって実行が失敗した場合`System.Reflection.TargetInvocationException was unhandled`、管理者特権で Visual Studio を再起動する必要があります。
8. 次のセクションに進む前にアプリケーションを停止します。

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>JavaScript クライアントとサーバーへのアクセス

同じ JavaScript クライアントから使用するこのセクションで、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)です。 1 つの変更はハブ URL を明示的に定義すると、クライアントにのみしましょう。 自己ホスト型アプリケーションでは、サーバー必ずしも (ため、リバース プロキシおよびロード バランサーを使用)、接続の URL と同じアドレスにあるためを明示的に定義する URL 必要があります。

1. **ソリューション エクスプ ローラー**ソリューションを右クリックし、選択、**追加**、**新しいプロジェクト**です。 選択、 **Web**ノードをクリックし、 **ASP.NET Web アプリケーション**テンプレート。 プロジェクト"JavascriptClient"の名前し、をクリックして**OK**です。

    ![](tutorial-signalr-self-host/_static/image3.png)
2. 選択、**空**テンプレート、および残りのオプションがオフのままにします。 選択**プロジェクトを作成する**です。

    ![](tutorial-signalr-self-host/_static/image4.png)
3. パッケージ マネージャー コンソールで、"JavascriptClient"でプロジェクトを選択、**既定のプロジェクト**ドロップダウンでは、次のコマンドを実行します。

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    このコマンドは、クライアントにする必要があります、SignalR と JQuery ライブラリをインストールします。
4. クリックして、プロジェクトを右クリックして**追加**、**新しい項目の**します。 選択、 **Web**ノード、および HTML ページを選択します。 ページの名前を付けます**Default.html**です。

    ![](tutorial-signalr-self-host/_static/image5.png)
5. 新しい HTML ページの内容を次のコードに置き換えます。 ここで、スクリプト参照がプロジェクトの Scripts フォルダー内のスクリプトを一致することを確認します。

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    (上記のコード例で強調表示されます)、次のコードは、(だけでなく、コードの SignalR バージョン 2 のベータ版にアップグレード) の起動を取得するチュートリアルで使用されるクライアントに対して行った追加です。 次のコード行は明示的に、サーバーで、SignalR の基本的な接続 URL を設定します。

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. ソリューションを右クリックし **スタートアップ プロジェクトを設定しています.**.選択、**マルチ スタートアップ プロジェクト**ラジオ ボタン、および両方のプロジェクトの設定**アクション**に**開始**です。

    ![](tutorial-signalr-self-host/_static/image6.png)
7. "Default.html"を右クリックし、選択**スタート ページとして設定**です。
8. アプリケーションを実行します。 サーバーおよびページが起動します。 Web ページを再読み込みする必要があります (または選択**続行**デバッガーで) サーバーが開始される前に、ページが読み込まれる場合。
9. ブラウザーで表示されたら、ユーザー名を提供します。 別のブラウザー タブまたはウィンドウに、ページの URL をコピーし、別のユーザー名を指定します。 チュートリアル入門のように、他の 1 つのブラウザー ウィンドウからメッセージを送信することができます。
