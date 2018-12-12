---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure Service Bus による SignalR スケール アウト (SignalR 1.x) |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 687d3d7787baa69410ee35d651a029c69d28c70b
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287001"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Azure Service Bus による SignalR スケール アウト (SignalR 1.x)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

このチュートリアルでは、Service Bus のバック プレーンを使用して各ロール インスタンスにメッセージを配信する、Windows Azure の Web ロールに SignalR アプリケーションを展開します。

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

必要条件:

- Windows Azure アカウント。
- [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409)します。
- Visual Studio 2012.

Service bus のバック プレーンと互換性のあるも[Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx)、バージョン 1.1。 ただし、Service Bus for Windows Server のバージョン 1.0 と互換性がありません。

## <a name="pricing"></a>Pricing

Service Bus のバック プレーンでは、トピックを使用して、メッセージを送信します。 最新の価格情報については、次を参照してください。 [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/)します。 この記事の執筆時に、1 ドル未満の 1 か月あたり 1,000,000 メッセージを送信できます。 バック プレーンでは、SignalR のハブ メソッドの呼び出しごとの service bus メッセージを送信します。 接続の切断、結合またはしたまま、やグループなどの一部のコントロール メッセージもあります。 ほとんどのアプリケーションでは、メッセージ トラフィックの大部分のハブ メソッド呼び出しとなります。

## <a name="overview"></a>概要

詳細なチュートリアルを始める前に、作業内容の簡単な概要を示します。

1. Windows Azure ポータルを使用すると、新しい Service Bus 名前空間を作成できます。
2. これらの NuGet パッケージをアプリケーションに追加します。 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. SignalR アプリケーションを作成します。
4. Global.asax バック プレーンを構成するには、次のコードを追加します。 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

各アプリケーションでは、"YourAppName"を別の値を選択します。 複数のアプリケーションでは、同じ値を使用しません。

## <a name="create-the-azure-services"></a>Azure サービスを作成します。

クラウド サービスを作成する」の説明に従って[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。 セクションの手順に従って"する方法。"簡易作成によるクラウド サービスを作成します。 このチュートリアルでは、証明書をアップロードする必要はありません。

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

新しい Service Bus 名前空間を作成する」の説明に従って[方法を使用して Service Bus トピック/サブスクリプション](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)します。 「Service Namespace を作成する」セクションの手順をに従います。

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> クラウド サービスと Service Bus 名前空間の同じリージョンを選択してください。


## <a name="create-the-visual-studio-project"></a>Visual Studio プロジェクトを作成します。

Visual Studio を起動します。 **ファイル** メニューのをクリックして**新しいプロジェクト**します。

**新しいプロジェクト** ダイアログ ボックスで、展開**Visual c#** します。 **インストールされたテンプレート**を選択します**クラウド**選び**Windows Azure クラウド サービス**します。 既定値を .NET Framework 4.5 を保持します。 ChatService アプリケーションの名前を指定し、をクリックして**OK**します。

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ASP.NET MVC 4 Web ロールを選択します。 右矢印ボタンをクリックします (**&gt;**)、ロールをソリューションに追加します。

マウスので、新しいロールでは、ポインターを置き、鉛筆アイコンが表示されます。 ロールの名前を変更するには、このアイコンをクリックします。 ロール"SignalRChat"という名前にして**OK**します。

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

**新しい ASP.NET MVC 4 プロジェクト**ウィザードで、**インターネット アプリケーション**します。 **[OK]** をクリックします。 プロジェクト ウィザードには、2 つのプロジェクトが作成されます。

- ChatService:このプロジェクトは、Windows Azure アプリケーションです。 Azure のロールとその他の構成オプションを定義します。
- SignalRChat:このプロジェクトは、ASP.NET MVC 4 プロジェクトです。

## <a name="create-the-signalr-chat-application"></a>SignalR チャット アプリケーションを作成します。

チャット アプリケーションを作成するには、チュートリアルの手順をに従って[SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)します。

必要なライブラリをインストールするのにには、NuGet を使用します。 **ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

使用して、 `-ProjectName` Windows Azure プロジェクトではなく、ASP.NET MVC プロジェクトにパッケージをインストールするオプション。

## <a name="configure-the-backplane"></a>バック プレーンを構成します。

アプリケーションの Global.asax ファイルでは、次のコードを追加します。

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Service bus の接続文字列を取得する必要があります。 Azure portal で作成した service bus 名前空間を選択し、アクセス キー アイコンをクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

接続文字列をクリップボードにコピーして貼り付けます、 *connectionString*変数。

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure に配置する

ソリューション エクスプ ローラーで、**ロール**ChatService プロジェクト内のフォルダー。

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

SignalRChat ロールを右クリックして**プロパティ**します。 選択、**構成**タブ。**インスタンス**2 を選択します。 VM のサイズを設定することもできます。**極小**します。

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

変更を保存します。

ソリューション エクスプ ローラーで、ChatService プロジェクトを右クリックします。 **[発行]** を選びます。

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Windows Azure に最初に公開する場合は、資格情報をダウンロードする必要があります。 **発行**ウィザード、[資格情報のダウンロードにサインイン] をクリックします。 これは、操作によって、Windows Azure ポータルにサインインし、発行設定ファイルをダウンロードするように求められます。

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

クリックして**インポート**ダウンロードした発行設定ファイルを選択します。

**[次へ]** をクリックします。 **発行設定**ダイアログで、**クラウド サービス**、先ほど作成したクラウド サービスを選択します。

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

**[発行]** をクリックします。 アプリケーションを展開し、Vm を起動するまで数分かかることができます。

今すぐチャット アプリケーションを実行するときに、ロール インスタンスは、Service Bus トピックを使用して、Azure Service Bus を介して通信します。 トピックは、複数のサブスクライバーを許可するメッセージ キューです。

バック プレーンは、トピックおよびサブスクリプションに自動的に作成されます。 サブスクリプションとメッセージ アクティビティを表示するには、Azure portal を開き、Service Bus 名前空間を選択して「トピック」をクリックします。

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

メッセージ アクティビティのダッシュ ボードに表示するまで数分かかることです。

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR では、トピックの「有効期間を管理します。 アプリケーションが展開されている限りは、トピックの設定を手動でのトピックを削除または変更しないでいます。
