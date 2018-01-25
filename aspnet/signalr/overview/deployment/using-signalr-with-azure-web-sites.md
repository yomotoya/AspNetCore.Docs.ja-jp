---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: "Azure App Service で Web アプリを使用して SignalR を使用して |Microsoft ドキュメント"
author: pfletcher
description: "このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。 ソフトウェアのバージョンは、Visual Studio 2013 または Vis. に、このチュートリアルで使用される."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Azure App Service で Web アプリを使用して SignalR を使用します。
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)または Visual Studio 2012
> - .NET 4.5
> - SignalR バージョン 2
> - Visual Studio 2013 または 2012 用の azure SDK 2.3
>   
> 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)、 [StackOverflow.com](http://stackoverflow.com/)、または[Microsoft Azure フォーラム](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>目次

- [はじめに](#introduction)
- [Azure App Service に SignalR Web アプリを展開します。](#deploying)
- [Azure App Service で Websocket を有効にします。](#websocket)
- [Azure Redis キャッシュのバック プレーンの使用](#backplane)
- [次の手順](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>はじめに

ASP.NET SignalR は、サーバーと web または .NET のクライアントの間の対話機能の新しいレベルを使用できます。 Azure でホストされるときに SignalR アプリケーションを活用すること、高可用性、拡張性が高く、し、パフォーマンスの高い環境、クラウドで実行されている提供します。

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Azure App Service に SignalR Web アプリを展開します。

SignalR はいない社内設置のサーバーへの展開とアプリケーションを Azure の配置に特定の混乱を追加します。 構成またはその他の設定は変更せず、SignalR を使用するアプリケーションを Azure でホストすることができます (ただし、Websocket のサポートを参照してください[Azure App Service で Websocket を有効にすると](#websocket)下です)。このチュートリアルで作成したアプリケーションを展開、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を Azure にします。

**必須コンポーネント**

- Visual Studio 2013. Visual Studio を持っていない場合、Azure SDK のインストールでは Visual Studio 2013 の Express for Web が含まれます。
- [Visual Studio 2013 用の azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)または[Visual Studio 2012 用の Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)です。
- このチュートリアルを完了するには、Azure サブスクリプションを必要があります。 実行できます[の MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)、または[試用版サブスクリプションにサインアップする](https://azure.microsoft.com/pricing/free-trial/)です。

### <a name="deploying-a-signalr-web-app-to-azure"></a>SignalR web アプリを Azure に展開します。

1. 完了、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したプロジェクトをダウンロードまたは[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)です。
2. Visual Studio で、次のように選択します。**ビルド**、**発行 SignalR チャット**です。
3. "Web の発行 ダイアログ ボックスで、「Windows Azure Web サイト」を選択します。

    ![Azure の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image1.png)
4. Microsoft アカウントにサインインしていない場合にをクリックして**サインインしています.** 「既存の Web サイトの選択」ダイアログでサインインします。

    ![既存の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure へのサインイン](using-signalr-with-azure-web-sites/_static/image3.png)
5. "既存の Web サイトの選択 ダイアログ ボックスで、をクリックして**新規**です。

    ![[新しい Web サイト]](using-signalr-with-azure-web-sites/_static/image4.png)
6. 「Windows Azure でサイトを作成する」ダイアログ ボックスで、一意のアプリ名を入力します。 領域のドロップダウン内で自分に最も近い地域を選択します。 **[作成]**をクリックします。

    ![Azure でサイトを作成します。](using-signalr-with-azure-web-sites/_static/image5.png)
7. "Web の発行 ダイアログ ボックスで、をクリックして**発行**です。

    ![サイトを発行します。](using-signalr-with-azure-web-sites/_static/image6.png)
8. アプリには、発行が完了したら、Azure App Service Web Apps でホストされている SignalR チャット アプリケーションはブラウザーで開きます。

    ![サイト、ブラウザーで開く](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Azure App Service Web Apps で Websocket を有効にします。

Websocket は、SignalR アプリケーションで使用する web アプリで明示的に有効にする必要があります。それ以外の場合、その他のプロトコルを使用する (を参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)詳細)。

Azure App Service Web Apps で Websocket を使用するためには、web アプリの構成セクションで有効にします。 これを行うには、web アプリを開く、 [Azure 管理ポータル](https://manage.windowsazure.com/)と構成 を選択します。

![[Configure (構成)] タブ](using-signalr-with-azure-web-sites/_static/image8.png)

[構成] ページの上部には、web アプリ用 .NET 4.5 が使用されることを確認します。

![.NET framework バージョン 4.5 の設定](using-signalr-with-azure-web-sites/_static/image9.png)

[構成] ページで、 **Websocket**選択を設定する**で**です。

![Websocket の設定: で](using-signalr-with-azure-web-sites/_static/image10.png)

[構成] ページの下部には、次のように選択します。**保存**して変更を保存します。

![設定を保存します。](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Azure Redis キャッシュのバック プレーンの使用

かどうか、web アプリの複数のインスタンスを使用して、それらのインスタンスのユーザーは、(、たとえば、1 つのインスタンスに作成されたチャット メッセージ到達できるように、他のインスタンスに接続しているユーザー) に、相互に対話する必要があります、 [Azure Redis Cacheバック プレーン](../performance/scaleout-with-redis.md)アプリケーションに実装する必要があります。

<a id="nextsteps"></a>
## <a name="next-steps"></a>次の手順

Azure App service Web Apps での詳細については、次を参照してください。 [Web アプリの概要](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)です。
