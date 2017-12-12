---
uid: signalr/overview/performance/scaleout-with-redis
title: "Redis で SignalR スケール アウト |Microsoft ドキュメント"
author: MikeWasson
description: "ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 965c32a4e2f2c9c4bd457d0c13ae99c1378c22c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis"></a>Redis で SignalR スケール アウト
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


このチュートリアルでは使用して[Redis](http://redis.io/) 2 つの独立した IIS インスタンス上に展開されている SignalR アプリケーション間でメッセージを配信します。

Redis は、メモリ内キー値ストアです。 また、モデルがパブリッシュ/サブスクライブ メッセージング システムをサポートします。 SignalR の Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。

![](scaleout-with-redis/_static/image1.png)

このチュートリアルでは、3 台のサーバーを使用します。

- SignalR アプリケーションを配置に使用する Windows を実行する 2 つのサーバー。
- Redis の実行に使用する、Linux を実行しているサーバーの 1 つです。 このチュートリアルのスクリーン ショット、Ubuntu 12.04 TLS を使用します。

使用する 3 つの物理サーバーを持っていない場合は、HYPER-V で Vm を作成できます。 Azure で Vm を作成することもできます。

このチュートリアルでは、公式の Redis 実装はまた、 [Windows ポートの Redis](https://github.com/MSOpenTech/redis) MSOpenTech からです。 セットアップおよび構成異なりますが、それ以外の場合、手順は同じです。

> [!NOTE] 
> 
> Redis で SignalR スケール アウトは、Redis クラスターをサポートしていません。


## <a name="overview"></a>概要

詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。

1. Redis をインストールし、Redis サーバーを起動します。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. SignalR アプリケーションを作成します。
4. Startup.cs バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>HYPER-V の Ubuntu

Windows HYPER-V を使用して、Windows Server 上の Ubuntu 仮想マシンを簡単に作成できます。

ダウンロードから ISO を Ubuntu [http://www.ubuntu.com](http://www.ubuntu.com/)です。

、HYPER-V では、新しい VM を追加します。 **仮想ハード_ディスクの接続**手順で、**仮想ハード_ディスクを作成する**です。

![](scaleout-with-redis/_static/image2.png)

**インストール オプション**手順で、**イメージ ファイル (.iso)**、 をクリックして**参照**、Ubuntu インストール ISO を参照します。

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Redis をインストールします。

手順に従う[http://redis.io/download](http://redis.io/download)をダウンロードし、Redis をビルドします。

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

これ、Redis のバイナリが構築、`src`ディレクトリ。

既定では、Redis では、パスワードは必要ありません。 パスワードを設定するには、編集、`redis.conf`ソース コードのルート ディレクトリにあるファイルです。 (バックアップ コピーを作成、ファイルの編集する前に!)次のディレクティブを追加`redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Redis サーバーを今すぐ開始するには。

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

開いているポート 6379 は Redis される既定のポートをリッスンします。 (構成ファイル内のポート番号を変更することができます)。

## <a name="create-the-signalr-application"></a>SignalR アプリケーションを作成します。

SignalR アプリケーションを作成するには、次のこれらのチュートリアルのいずれか。

- [SignalR 2.0 の概要](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

次に、Redis でスケール アウトをサポートするためにチャット アプリケーションを変更します。 まず、SignalR.Redis NuGet パッケージをプロジェクトに追加します。 Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

次に、Startup.cs ファイルを開きます。 次のコードを追加、**構成**メソッド。

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server"は、Redis を実行しているサーバーの名前です。
- *ポート*のポート番号です
- "password"は、redis.conf ファイルで定義されているパスワードです。
- "AppName"は、任意の文字列です。 SignalR では、この名前の Redis のパブリッシュ/サブスクライブ チャネルを作成します。

例:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>展開し、アプリケーションを実行

SignalR アプリケーションを展開する、Windows Server のインスタンスを準備します。

IIS の役割を追加します。 WebSocket プロトコルを含む、「アプリケーション開発」機能が含まれます。

![](scaleout-with-redis/_static/image5.png)

([管理ツール] 下に表示)、管理サービスがあります。

![](scaleout-with-redis/_static/image6.png)

**インストールの Web Deploy 3.0。** IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求め ことができます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)です。 Platform Installer で Web Deploy を検索し、Web Deploy 3.0 をインストール

![](scaleout-with-redis/_static/image7.png)

Web 管理サービスが実行されていることを確認してください。 それ以外の場合は、サービスを開始します。 (Web 管理サービスは、Windows サービスの一覧に表示されない場合、は、IIS の役割を追加するときに、管理サービスがインストールされていることを確認) します。

既定では、Web 管理サービスは TCP ポート 8172 でリッスンします。 Windows ファイアウォールでポート 8172 で TCP トラフィックを許可する場合は、新しい受信規則を作成します。 詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx)です。 (Azure 上の Vm をホストしている場合ことがこれを行う、Azure ポータルで直接です。 参照してください[仮想マシンにエンドポイントを設定する方法](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/))。

Visual Studio プロジェクトを開発用コンピューターからサーバーを配置する準備が整いました。 ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**です。

Web 展開に関するドキュメントの詳細を参照してください。 [Visual Studio と ASP.NET の Web 展開コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)です。

2 つのサーバーにアプリケーションを展開する場合は、別のブラウザー ウィンドウで各インスタンスを開くし、一方の SignalR メッセージを受信互いを参照してください。 (当然ながら、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)

![](scaleout-with-redis/_static/image8.png)

使用することができます、Redis に送信されるメッセージを表示する興味があるなら、 **redis cli** Redis と共にインストールされるクライアント。

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
