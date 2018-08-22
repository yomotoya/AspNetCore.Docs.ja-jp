---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Azure Service Bus による SignalR スケール アウト |Microsoft Docs
author: MikeWasson
description: ソフトウェアのバージョンは、このトピックの「Visual Studio 2013 .NET 4.5 SignalR バージョン 2 以前のバージョンのこのトピックでは、このトピック SignalR の 1.x バージョンのを使用しています.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b87eb9f2df82d92c07ea0c86873849a44660e5c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834334"
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Azure Service Bus による SignalR スケール アウト
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

このチュートリアルでは、Service Bus のバック プレーンを使用して各ロール インスタンスにメッセージを配信する、Windows Azure の Web ロールに SignalR アプリケーションを展開します。 (Service Bus のバック プレーンとを使用することもできます[web アプリを Azure App Service で](https://docs.microsoft.com/azure/app-service-web/)。)。

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

必要条件:

- Windows Azure アカウント。
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)します。
- Visual Studio 2012 または 2013。

Service bus のバック プレーンと互換性のあるも[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)、バージョン 1.1。 ただし、Service Bus for Windows Server のバージョン 1.0 と互換性がありません。

## <a name="pricing"></a>Pricing

Service Bus のバック プレーンでは、トピックを使用して、メッセージを送信します。 最新の価格情報については、次を参照してください。 [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)します。 この記事の執筆時に、1 ドル未満の 1 か月あたり 1,000,000 メッセージを送信できます。 バック プレーンでは、SignalR のハブ メソッドの呼び出しごとの service bus メッセージを送信します。 接続の切断、結合またはしたまま、やグループなどの一部のコントロール メッセージもあります。 ほとんどのアプリケーションでは、メッセージ トラフィックの大部分のハブ メソッド呼び出しとなります。

## <a name="overview"></a>概要

詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。

1. Windows Azure ポータルを使用すると、新しい Service Bus 名前空間を作成できます。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)または[Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. SignalR アプリケーションを作成します。
4. Startup.cs バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

このコードでは、バック プレーンを構成の既定値を持つ[TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx)と[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx)します。 これらの値を変更する方法については、次を参照してください。 [SignalR パフォーマンス: スケール アウト メトリック](signalr-performance.md#scaleout_metrics)します。

各アプリケーションでは、"YourAppName"を別の値を選択します。 複数のアプリケーションでは、同じ値を使用しません。

## <a name="create-the-azure-services"></a>Azure サービスを作成します。

クラウド サービスを作成する」の説明に従って[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。 セクションの手順に従って"する方法: 簡易作成を使用してクラウド サービスの作成"します。 このチュートリアルでは、証明書をアップロードする必要はありません。

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

新しい Service Bus 名前空間を作成する」の説明に従って[方法を使用して Service Bus トピック/サブスクリプション](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)します。 「Service Namespace を作成する」セクションの手順をに従います。

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> クラウド サービスと Service Bus 名前空間の同じリージョンを選択してください。


## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio を起動します。 **ファイル** メニューのをクリックして**新しいプロジェクト**します。

**新しいプロジェクト** ダイアログ ボックスで、展開**Visual c#** します。 **インストールされたテンプレート**を選択します**クラウド**選び**Windows Azure クラウド サービス**します。 既定値を .NET Framework 4.5 を保持します。 ChatService アプリケーションの名前を指定し、をクリックして**OK**します。

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ASP.NET Web ロールを選択します。 右矢印ボタンをクリックします (**&gt;**)、ロールをソリューションに追加します。

マウスので、新しいロールでは、ポインターを置き、鉛筆アイコンが表示されます。 ロールの名前を変更するには、このアイコンをクリックします。 ロール"SignalRChat"という名前にして**OK**します。

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**、[ok] をクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

プロジェクト ウィザードには、2 つのプロジェクトが作成されます。

- ChatService: このプロジェクトは、Windows Azure アプリケーションです。 Azure のロールとその他の構成オプションを定義します。
- SignalRChat: このプロジェクトは、ASP.NET MVC 5 プロジェクトです。

## <a name="create-the-signalr-chat-application"></a>SignalR チャット アプリケーションを作成します。

チャット アプリケーションを作成するには、チュートリアルの手順をに従って[SignalR と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)します。

必要なライブラリをインストールするのにには、NuGet を使用します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

使用して、 `-ProjectName` Windows Azure プロジェクトではなく、ASP.NET MVC プロジェクトにパッケージをインストールするオプション。

## <a name="configure-the-backplane"></a>バック プレーンを構成します。

アプリケーションの Startup.cs ファイルでは、次のコードを追加します。

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Service bus の接続文字列を取得する必要があります。 Azure portal で作成した service bus 名前空間を選択し、アクセス キー アイコンをクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

接続文字列をクリップボードにコピーして貼り付けます、 *connectionString*変数。

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure に配置する

ソリューション エクスプ ローラーで、**ロール**ChatService プロジェクト内のフォルダー。

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

SignalRChat ロールを右クリックして**プロパティ**します。 選択、**構成**タブ。**インスタンス**2 を選択します。 VM のサイズを設定することもできます。**極小**します。

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

変更を保存します。

ソリューション エクスプ ローラーで、ChatService プロジェクトを右クリックします。 **[発行]** を選びます。

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Windows Azure に最初に公開する場合は、資格情報をダウンロードする必要があります。 **発行**ウィザード、[資格情報のダウンロードにサインイン] をクリックします。 これは、操作によって、Windows Azure ポータルにサインインし、発行設定ファイルをダウンロードするように求められます。

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

クリックして**インポート**ダウンロードした発行設定ファイルを選択します。

**[次へ]** をクリックします。 **発行設定**ダイアログで、**クラウド サービス**、先ほど作成したクラウド サービスを選択します。

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

**[発行]** をクリックします。 アプリケーションを展開し、Vm を起動するまで数分かかることができます。

今すぐチャット アプリケーションを実行するときに、ロール インスタンスは、Service Bus トピックを使用して、Azure Service Bus を介して通信します。 トピックは、複数のサブスクライバーを許可するメッセージ キューです。

バック プレーンは、トピックおよびサブスクリプションに自動的に作成されます。 サブスクリプションとメッセージ アクティビティを表示するには、Azure portal を開き、Service Bus 名前空間を選択して「トピック」をクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

メッセージ アクティビティのダッシュ ボードに表示するまで数分かかることです。

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR では、トピックの「有効期間を管理します。 アプリケーションが展開されている限りは、トピックの設定を手動でのトピックを削除または変更しないでいます。

## <a name="troubleshooting"></a>トラブルシューティング

**System.InvalidOperationException「唯一サポートされている IsolationLevel が 'IsolationLevel.Serializable'.」**

このエラーは、操作のトランザクション レベルが以外のものに設定されている場合に発生する可能性が`Serializable`します。 他のトランザクション レベルで操作が実行されるしないことを確認します。
