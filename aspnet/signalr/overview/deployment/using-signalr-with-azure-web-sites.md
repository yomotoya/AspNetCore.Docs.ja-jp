---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: SignalR を使用して Azure App Service で Web アプリの使用 |Microsoft Docs
author: pfletcher
description: このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。 ソフトウェアのバージョンは、Visual Studio 2013 または Vis. チュートリアルで使用.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: dabf0f6cfed401e10d2c1134c260022d94c3ab92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389576"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Azure App Service で Web アプリで SignalR を使用
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)または Visual Studio 2012
> - .NET 4.5
> - SignalR 2 のバージョン
> - Visual Studio 2013 または 2012 用の azure SDK 2.3
>   
> 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)、 [StackOverflow.com](http://stackoverflow.com/)、または[Microsoft Azure フォーラム](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>目次

- [はじめに](#introduction)
- [Azure App Service に SignalR Web アプリを展開します。](#deploying)
- [Azure App Service で Websocket を有効にします。](#websocket)
- [Azure Redis Cache のバック プレーンを使用します。](#backplane)
- [次の手順](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>はじめに

ASP.NET SignalR は、新しいレベルのサーバーと web または .NET クライアント間の対話機能を使用できます。 Azure でホストされているときに SignalR アプリケーションを利用して高可用性のスケーラブルなおよびパフォーマンスの高い環境を提供する、クラウドで実行されています。

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Azure App Service に SignalR Web アプリを展開します。

SignalR を Azure にアプリケーションを展開すると、オンプレミス サーバーにデプロイする特定の混乱を追加しません。 構成またはその他の設定に変更を行わず、SignalR を使用するアプリケーションを Azure でホストされることができます (ただし Websocket サポートについてを参照してください[Azure App Service で Websocket を有効にする](#websocket)以下です)。このチュートリアルで作成したアプリケーションをデプロイします、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を Azure にします。

**必須コンポーネント**

- Visual Studio 2013. Visual Studio を持っていない場合、Azure SDK のインストールでは Visual Studio 2013 の Express for Web が含まれます。
- [Visual Studio 2013 向け azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)または[Visual Studio 2012 用の Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)します。
- このチュートリアルを完了するには、Azure サブスクリプションを必要があります。 できます[MSDN サブスクリプション会員の特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)、または[試用版サブスクリプションにサインアップ](https://azure.microsoft.com/pricing/free-trial/)します。

### <a name="deploying-a-signalr-web-app-to-azure"></a>SignalR の web アプリを Azure に展開します。

1. 完了、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したプロジェクトをダウンロードまたは[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)します。
2. Visual Studio で、次のように選択します。**ビルド**、 **SignalR チャットの発行**します。
3. "Web の発行 ダイアログ ボックスで、「Windows Azure の Web サイト」を選択します。

    ![Azure の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image1.png)
4. Microsoft アカウントにサインインしていない場合はクリックして**サインインしています.** で [既存の Web サイトの選択] ダイアログ ボックスで、サインインします。

    ![既存の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure にサインインする](using-signalr-with-azure-web-sites/_static/image3.png)
5. [既存の Web サイトの選択] ダイアログ ボックスで、次のようにクリックします。**新規**します。

    ![[新しい Web サイト]](using-signalr-with-azure-web-sites/_static/image4.png)
6. "Windows Azure でサイトの作成 ダイアログ ボックスで、一意のアプリ名を入力します。 領域のドロップダウン リストでユーザーに最も近いリージョンを選択します。 **[作成]** をクリックします。

    ![Azure でサイトを作成します。](using-signalr-with-azure-web-sites/_static/image5.png)
7. "Web の発行 ダイアログ ボックスで、次のようにクリックします。**発行**します。

    ![サイトを発行します。](using-signalr-with-azure-web-sites/_static/image6.png)
8. アプリの発行が完了したら、SignalR チャット アプリケーションを Azure App Service Web Apps でホストされているが、ブラウザーで開きます。

    ![サイトがブラウザーで開く](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Azure App Service Web Apps で Websocket を有効にします。

Websocket は、SignalR アプリケーションで使用する web アプリで明示的に有効にする必要があります。それ以外の場合、他のプロトコルを使用する (を参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)詳細については)。

Azure App Service Web Apps で Websocket を使用するためには、web アプリの構成セクションで有効にします。 これを行うには、web アプリを開き、 [Azure 管理ポータル](https://manage.windowsazure.com/)構成を選択します。

![[Configure (構成)] タブ](using-signalr-with-azure-web-sites/_static/image8.png)

構成ページの上部にある、web アプリ用 .NET 4.5 が使用されることを確認します。

![.NET framework バージョン 4.5 の設定](using-signalr-with-azure-web-sites/_static/image9.png)

[構成] ページで、 **Websocket**設定で選択**で**します。

![Websocket の設定: 上](using-signalr-with-azure-web-sites/_static/image10.png)

[構成] ページの下部には、次のように選択します。**保存**、変更を保存します。

![設定を保存します。](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Azure Redis Cache のバック プレーンを使用します。

Web アプリの複数のインスタンスを使用して、それらのインスタンスのユーザー間の対話 (つまり、たとえば、1 つのインスタンスに作成されたチャット メッセージは、他のインスタンスに接続しているユーザーにアクセスできる) する必要があるかどうか、 [Azure Redis Cacheバック プレーン](../performance/scaleout-with-redis.md)アプリケーションで実装する必要があります。

<a id="nextsteps"></a>
## <a name="next-steps"></a>次の手順

Azure App Service で Web アプリの詳細については、次を参照してください。 [Web Apps の概要](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)します。
