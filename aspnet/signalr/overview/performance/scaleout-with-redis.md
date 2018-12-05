---
uid: signalr/overview/performance/scaleout-with-redis
title: Redis による SignalR スケール アウト |Microsoft Docs
author: MikeWasson
description: このトピックの「Visual Studio 2013 .NET 4.5 SignalR 使用されるソフトウェアのバージョンは以前のバージョンについてはこのトピック以前バージョンをバージョン 2.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: f92946ae99bf8cb3840adb5d98004acb87e24925
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861291"
---
<a name="signalr-scaleout-with-redis"></a>Redis による SignalR スケール アウト
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR バージョン 2.4
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
>
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
>
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。


このチュートリアルでは使用して[Redis](http://redis.io/) 2 つの IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配信します。

Redis はメモリ内のキー値ストアです。 パブリッシュ/サブスクライブ モデルでのメッセージング システムもサポートしています。 SignalR で Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。

![](scaleout-with-redis/_static/image1.png)

このチュートリアルでは、3 台のサーバーを使用します。

- SignalR アプリケーションをデプロイするのに使用する、Windows を実行している 2 台のサーバー。
- Redis の実行に使用するには、Linux を実行しているサーバーの 1 つ。 このチュートリアルではスクリーン ショットは、Ubuntu 12.04 TLS を使用しました。

使用する 3 つの物理サーバーを持っていない場合は、HYPER-V で Vm を作成できます。 別のオプションでは、Azure で Vm を作成します。

このチュートリアルでは、公式の Redis の実装はまた、 [Redis のポートを Windows](https://github.com/MSOpenTech/redis) MSOpenTech から。 セットアップと構成が異なるが、それ以外の場合、手順は同じです。

> [!NOTE]
>
> Redis による SignalR スケール アウトは、Redis クラスターをサポートしていません。


## <a name="overview"></a>概要

詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。

1. Redis をインストールし、Redis サーバーを起動します。
2. これらの NuGet パッケージをアプリケーションに追加します。

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. SignalR アプリケーションを作成します。
4. Startup.cs バック プレーンを構成するには、次のコードを追加します。

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Hyper V 上の Ubuntu

Windows HYPER-V を使用して、Windows Server 上の Ubuntu VM を簡単に作成できます。

ISO をダウンロードする Ubuntu から[ http://www.ubuntu.com](http://www.ubuntu.com/)します。

HYPER-V では、新しい VM を追加します。 **仮想ハード_ディスクの接続**手順で、**仮想ハード ディスクを作成する**します。

![](scaleout-with-redis/_static/image2.png)

**インストール オプション**手順で、**イメージ ファイル (.iso)**、 をクリックして**参照**Ubuntu のインストール ISO を参照します。

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Redis をインストールします。

手順に従う[ http://redis.io/download ](http://redis.io/download)をダウンロードして、Redis をビルドします。

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Redis バイナリ ビルド、`src`ディレクトリ。

既定では、Redis では、パスワードは必要ありません。 パスワードを設定するには、編集、`redis.conf`ファイルで、ソース コードのルート ディレクトリにあります。 (バックアップ コピーを作成、ファイルの編集する前に!)次のディレクティブを追加`redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Redis サーバーを今すぐ開始するには。

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Redis の既定のポートが開かれたポート 6379 をリッスンします。 (構成ファイルでポート番号を変更できます)。

## <a name="create-the-signalr-application"></a>SignalR アプリケーションを作成します。

これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。

- [SignalR 2.0 の概要](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

次に、私たち Redis によるスケール アウトをサポートするために、チャット アプリケーションを変更します。 最初に、追加、`Microsoft.AspNet.SignalR.StackExchangeRedis`をプロジェクトに NuGet パッケージ。 Visual Studio から、**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

次に、Startup.cs ファイルを開きます。 次のコードを追加、**構成**メソッド。

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server"は、Redis を実行しているサーバーの名前です。
- *ポート*ポート番号
- "password"では、redis.conf についてファイルで定義されているパスワードです。
- "AppName"は、任意の文字列です。 SignalR では、この名前の Redis のパブリッシュ/サブスクライブ チャンネルを作成します。

例えば:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>展開し、アプリケーションを実行

SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。

IIS の役割を追加します。 WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。

![](scaleout-with-redis/_static/image5.png)

管理サービス ([管理ツール] の下に表示) とも含まれます。

![](scaleout-with-redis/_static/image6.png)

**インストール Web Deploy 3.0 です。** IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。 プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール

![](scaleout-with-redis/_static/image7.png)

Web 管理サービスが実行されていることを確認します。 それ以外の場合は、サービスを開始します。 (Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。

既定では、Web 管理サービスは TCP ポート 8172 でリッスンします。 Windows ファイアウォールでポート 8172 で TCP トラフィックを許可する場合は、新しい受信規則を作成します。 詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)します。 (Azure 上の Vm をホストしている場合はこれを行う、Azure portal で直接します。 参照してください[仮想マシンにエンドポイントを設定する方法](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/))。

サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。 ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。

Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。

2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。 (もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)

![](scaleout-with-redis/_static/image8.png)

使用することができます、Redis に送信されるメッセージを表示する関心がある場合、 **redis cli**クライアントで、Redis と共にインストールされます。

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
