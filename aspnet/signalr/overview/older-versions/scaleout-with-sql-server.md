---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server による SignalR スケール アウト (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: cd0e3d4bdb4d2eb78e5c41167a17f8673584f654
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826272"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SQL Server による SignalR スケール アウト (SignalR 1.x)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

このチュートリアルでは、2 つの IIS インスタンスで配置されている SignalR アプリケーション間でメッセージを配布するのに SQL Server を使用します。 1 つのテスト コンピューターで、このチュートリアルを実行することもできますが、完全な効果を取得するには SignalR アプリケーションを 2 つまたは複数のサーバーをデプロイする必要があります。 サーバーのいずれか、または別の専用サーバーでは、SQL Server をインストールすることもする必要があります。 別のオプションでは、Azure で Vm を使用してチュートリアルを実行します。

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>必須コンポーネント

Microsoft SQL Server 2005 以降。 バック プレーンには、SQL Server のデスクトップとサーバーの両方のエディションがサポートされています。 SQL Server Compact Edition または Azure SQL Database はサポートされません。 (場合は、アプリケーションは、Azure でホストされる、Service Bus のバック プレーン代わりに、検討します。)

## <a name="overview"></a>概要

詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。

1. 新しい空のデータベースを作成します。 バック プレーンはこのデータベースに必要なテーブルを作成します。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. SignalR アプリケーションを作成します。
4. Global.asax バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>データベースを構成します。

かどうか、アプリケーションが Windows 認証または SQL Server 認証に使用、データベースへのアクセスを決定します。 関係なく、データベース ユーザーにログインし、スキーマを作成して、テーブルを作成するアクセス許可を確認します。

使用するバック プレーンの新しいデータベースを作成します。 データベースに任意の名前を付けることができます。 データベースでは、テーブルを作成する必要はありません。バック プレーンでは、必要なテーブルを作成します。

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Service Broker を有効にします。

バック プレーンのデータベースの Service Broker を有効にすることをお勧めします。 Service Broker では、メッセージングとバック プレーンのより効率的に更新プログラムを受信できるように SQL Server でのキューのネイティブ サポートを提供します。 (ただし、バック プレーンもさせずに Service Broker。)

Service Broker が有効になっているかどうかを確認するには、クエリ、**は\_broker\_有効になっている**内の列、 **sys.databases**カタログ ビューです。

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Service Broker を有効にするには、次の SQL クエリを使用します。

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> このクエリは、デッドロックを確認が表示された場合、DB に接続されているアプリケーションはありません。


トレースを有効にした場合、トレースが表示されます Service Broker が有効になっているかどうか。

## <a name="create-a-signalr-application"></a>SignalR アプリケーションを作成します。

これらのチュートリアルのいずれかを次で SignalR アプリケーションを作成します。

- [SignalR の概要](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)

次に、私たちと SQL Server のスケール アウトをサポートするために、チャット アプリケーションを変更します。 まず、SignalR.SqlServer NuGet パッケージをプロジェクトに追加します。 Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

次に、Global.asax ファイルを開きます。 次のコードを追加、**アプリケーション\_開始**メソッド。

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>展開し、アプリケーションを実行

SignalR アプリケーションをデプロイするには、Windows Server インスタンスを準備します。

IIS の役割を追加します。 WebSocket プロトコルを含む、「アプリケーションの開発」機能が含まれます。

![](scaleout-with-sql-server/_static/image4.png)

管理サービス ([管理ツール] の下に表示) とも含まれます。

![](scaleout-with-sql-server/_static/image5.png)

**インストール Web Deploy 3.0 です。** IIS マネージャーを実行するとき、Microsoft Web プラットフォームをインストールするように求められますできます[ダウンロード、intstaller](https://go.microsoft.com/fwlink/?LinkId=255386)します。 プラットフォーム インストーラーで Web Deploy を検索し、Web Deploy 3.0 をインストール

![](scaleout-with-sql-server/_static/image6.png)

Web 管理サービスが実行されていることを確認します。 それ以外の場合は、サービスを開始します。 (Web 管理サービスで Windows サービスの一覧が表示されない場合は、IIS の役割を追加したときに、管理サービスがインストールされていることを確認)。

最後に、tcp ポート 8172 を開きます。 これは、Web 配置ツールで使用されるポートです。

サーバーに、開発コンピューターから Visual Studio プロジェクトを配置する準備が整いました。 ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックして**発行**します。

Web デプロイに関するドキュメントの詳細を参照してください。 [for Visual Studio および ASP.NET の Web 配置コンテンツ マップ](../../../whitepapers/aspnet-web-deployment-content-map.md)します。

2 つのサーバー アプリケーションをデプロイする場合は、別のブラウザー ウィンドウで各インスタンスを開くし、もう一方の SignalR メッセージを受信互いを参照してください。 (もちろん、実稼働環境で 2 つのサーバー放置ロード バランサーの背後にします。)

![](scaleout-with-sql-server/_static/image7.png)

アプリケーションを実行した後、SignalR がデータベース内でテーブルを作成が自動的にことが確認できます。

![](scaleout-with-sql-server/_static/image8.png)

SignalR は、テーブルを管理します。 アプリケーションが展開されている限りしない行を削除、変更、テーブルのなど。
