---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure Service Bus での SignalR スケール アウト (SignalR 1.x) |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Azure Service Bus での SignalR スケール アウト (SignalR 1.x)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

このチュートリアルでは、Service Bus バック プレーンを使用して各ロール インスタンスにメッセージを配信する、Windows Azure の Web ロールに SignalR アプリケーションを展開します。

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

必要条件:

- Windows Azure アカウント。
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)です。
- Visual Studio 2012.

サービス バスのバック プレーンに互換性がも[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)、version 1.1 です。 ただし、Service Bus for Windows Server のバージョン 1.0 と互換性がありません。

## <a name="pricing"></a>Pricing

Service Bus バック プレーンでは、トピックを使用して、メッセージを送信します。 最新の価格情報については、次を参照してください。 [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)です。 この記事の執筆時に、1 ドル未満の値の 1 か月あたり 1,000,000 メッセージを送信できます。 バック プレーンでは、SignalR のハブ メソッドの呼び出しごとに service bus メッセージを送信します。 接続、切断、または参加したまま、グループなどの一部のコントロール メッセージもあります。 ほとんどのアプリケーションでは、メッセージ トラフィックの大部分のハブ メソッド呼び出しとなります。

## <a name="overview"></a>概要

詳細なチュートリアルを始める前に何を行うの簡単な概要を次に示します。

1. Windows Azure ポータルを使用して、新しい Service Bus 名前空間を作成します。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. SignalR アプリケーションを作成します。
4. Global.asax バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

各アプリケーションには、"YourAppName"の別の値を選択します。 複数のアプリケーションは、同じ値を使用しないでください。

## <a name="create-the-azure-services"></a>Azure のサービスを作成します。

クラウド サービスを作成する」の説明に従って[を作成し、クラウド サービスを展開する方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)です。 セクションの手順に従って"する方法: 簡易作成を使用してクラウド サービスの作成"です。 このチュートリアルでは、証明書をアップロードする必要はありません。

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

新しい Service Bus 名前空間を作成する」の説明に従って[方法を使用するサービス バス トピック/サブスクリプション](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)です。 「Service Namespace を作成する」セクションの手順をに従います。

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> クラウド サービスと Service Bus 名前空間の同じ領域を選択してください。


## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio を起動します。 **ファイル** メニューをクリックして**新しいプロジェクト**です。

**新しいプロジェクト** ダイアログ ボックスで、展開**Visual c#** です。 **インストールされたテンプレート****クラウド**し、 **Windows Azure クラウド サービス**です。 既定値を .NET Framework 4.5 を保持します。 ChatService アプリケーションの名前を指定し、をクリックして**OK**です。

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

**新しい Windows Azure のクラウド サービス**ダイアログ ボックスで、ASP.NET MVC 4 Web ロールを選択します。 右矢印ボタンをクリックして (**&gt;**) をソリューションにロールを追加します。

マウス ポインター新しいロールをそのため、鉛筆アイコンが表示されます。 ロールの名前を変更するには、このアイコンをクリックします。 "SignalRChat"ロールの名前をクリックして**OK**です。

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

**新しい ASP.NET MVC 4 プロジェクト**ウィザードで、**インターネット アプリケーション**です。 **[OK]** をクリックします。 チーム プロジェクト ウィザードでは、2 つのプロジェクトを作成します。

- ChatService: このプロジェクトは、Windows Azure アプリケーションです。 これは、Azure のロールとその他の構成オプションを定義します。
- SignalRChat: このプロジェクトは、ASP.NET MVC 4 プロジェクトです。

## <a name="create-the-signalr-chat-application"></a>SignalR チャット アプリケーションを作成します。

チャット アプリケーションを作成する手順のチュートリアルで[SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)です。

NuGet を使用すると、必要なライブラリをインストールできます。 **ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

使用して、 `-ProjectName` Windows Azure プロジェクトではなく、ASP.NET MVC プロジェクトにパッケージをインストールするオプションです。

## <a name="configure-the-backplane"></a>バック プレーンを構成します。

アプリケーションの Global.asax ファイルでは、次のコードを追加します。

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Service bus の接続文字列を取得する必要があります。 Azure ポータルで作成した service bus 名前空間を選択し、アクセス キー アイコンをクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

接続文字列をクリップボードにコピーして貼り付けます、 *connectionString*変数。

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure への配置します。

ソリューション エクスプ ローラーで、展開、**ロール**ChatService プロジェクト内のフォルダーです。

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

SignalRChat ロールを右クリックし **プロパティ**です。 選択、**構成**タブです。**インスタンス**2 を選択します。 VM のサイズを設定することもできます。**極小**です。

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

変更を保存します。

ソリューション エクスプ ローラーで、ChatService プロジェクトを右クリックします。 **[発行]** を選びます。

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Windows Azure に最初に発行する、この場合は、資格情報をダウンロードする必要があります。 **発行**ウィザード、[資格情報をダウンロードするサインイン] をクリックします。 これは、操作によって、Windows Azure ポータルにサインインし、発行設定ファイルをダウンロードするように求められます。

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

をクリックして**インポート**をダウンロードした発行設定ファイルを選択します。

**[次へ]** をクリックします。 **発行設定**ダイアログで、**クラウド サービス**、先ほど作成したクラウド サービスを選択します。

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

**[発行]** をクリックします。 アプリケーションを展開し、Vm を起動するまで数分かかることができます。

今すぐチャット アプリケーションを実行すると、ロール インスタンスは、Service Bus トピックを使用して、Azure Service Bus を通じて通信します。 トピックは、複数のサブスクライバーを許可するメッセージ キューです。

バック プレーンには、トピックとサブスクリプションを自動的に作成されます。 サブスクリプションとメッセージ アクティビティを表示するには、Azure ポータルを開き、Service Bus 名前空間を選択および「トピック」をクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

これは、ダッシュ ボードに表示するメッセージ アクティビティに対して数分かかるためです。

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR では、トピックの有効期間を管理します。 アプリケーションが展開されている限りは、トピックの設定を手動でトピックの削除または変更しないでください。
