---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "SQL Server での SignalR スケール アウト |Microsoft ドキュメント"
author: MikeWasson
description: "ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a>SQL Server での SignalR スケール アウト
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


このチュートリアルでは SQL Server を使用して 2 つの独立した IIS インスタンスに配置されている SignalR アプリケーション間でメッセージを配布します。 1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、効果を得るには、次の 2 つまたは複数のサーバーに SignalR アプリケーションを配置する必要があります。 いずれかのサーバーまたは別の専用サーバーでは、SQL Server をインストールすることも必要があります。 Azure で Vm を使用してチュートリアルを実行することもできます。

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>必須コンポーネント

Microsoft SQL Server 2005 以降。 バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。 これは、SQL Server Compact Edition または Azure SQL Database にはサポートしません。 (アプリケーションが Azure でホストされている場合を検討 Service Bus バック プレーン代わりにします。)

## <a name="overview"></a>概要

詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。

1. 新しい空のデータベースを作成します。 バック プレーンには、このデータベースに必要なテーブルは作成します。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. SignalR アプリケーションを作成します。
4. Startup.cs バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 このコードの既定値でのバック プレーンの構成[TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx)と[MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)です。 これらの値を変更する方法については、次を参照してください。 [SignalR パフォーマンス: スケール アウト メトリック](signalr-performance.md#scaleout_metrics)です。 

## <a name="configure-the-database"></a>データベースを構成します。

かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。 関係なく、データベース ユーザーがログイン、スキーマを作成し、テーブルを作成するアクセス許可を確認します。

使用するバック プレーンの新しいデータベースを作成します。 データベースには、任意の名前を付けることができます。 データベースの任意のテーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Service Broker を有効にします。

バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。 Service Broker は、メッセージングとバック プレーンの更新プログラムをより効率的に受信できるようにする SQL Server のキューに対するネイティブ サポートを提供します。 (ただし、バック プレーンでも Service Broker なし。)

Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Service Broker を有効にするには、次の SQL クエリを使用します。

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> このクエリは、デッドロックを起こすことを確認するが表示された場合、DB に接続されているアプリケーションはありません。


トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。

## <a name="create-a-signalr-application"></a>SignalR アプリケーションを作成します。

SignalR アプリケーションを作成するには、次のこれらのチュートリアルのいずれか。

- [SignalR 2.0 の概要](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR 2.0 と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

次に、SQL Server でスケール アウトをサポートするためにチャット アプリケーションを変更します。 まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。 Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

次に、Startup.cs ファイルを開きます。 次のコードを追加、**構成**メソッド。

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>展開し、アプリケーションを実行

SignalR アプリケーションを展開する、Windows Server のインスタンスを準備します。

IIS の役割を追加します。 WebSocket プロトコルを含む、「アプリケーション開発」機能が含まれます。

![](scaleout-with-sql-server/_static/image4.png)

([管理ツール] 下に表示)、管理サービスがあります。

![](scaleout-with-sql-server/_static/image5.png)

**インストールの Web Deploy 3.0。** IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求め ことができます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)です。 Platform Installer で Web Deploy を検索し、Web Deploy 3.0 をインストール

![](scaleout-with-sql-server/_static/image6.png)

Web 管理サービスが実行されていることを確認してください。 それ以外の場合は、サービスを開始します。 (Web 管理サービスは、Windows サービスの一覧に表示されない場合、は、IIS の役割を追加するときに、管理サービスがインストールされていることを確認) します。

最後に、TCP のポート 8172 を開きます。 これは、Web Deploy ツールを使用するポートです。

Visual Studio プロジェクトを開発用コンピューターからサーバーを配置する準備が整いました。 ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**です。

Web 展開に関するドキュメントの詳細を参照してください。 [Visual Studio と ASP.NET の Web 展開コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)です。

2 つのサーバーにアプリケーションを展開する場合は、別のブラウザー ウィンドウで各インスタンスを開くし、一方の SignalR メッセージを受信互いを参照してください。 (当然ながら、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)

![](scaleout-with-sql-server/_static/image7.png)

アプリケーションを実行した後は、SignalR が自動的に作成されたことのテーブル、データベース内を確認できます。

![](scaleout-with-sql-server/_static/image8.png)

SignalR では、テーブルを管理します。 アプリケーションが展開されている限り、しない行を削除、テーブルの変更など。
