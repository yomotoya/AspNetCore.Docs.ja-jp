---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: ホストする Azure Worker ロールで OWIN |Microsoft ドキュメント
author: MikeWasson
description: このチュートリアルでは、Microsoft Azure ワーカー ロールで OWIN を自己ホストする方法を示します。 Open Web Interface の .NET (OWIN) では、.NET の web サーバー間の抽象化を定義しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868426"
---
<a name="host-owin-in-an-azure-worker-role"></a>ホストする Azure Worker ロールで OWIN
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

> このチュートリアルでは、Microsoft Azure ワーカー ロールで OWIN を自己ホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。 OWIN OWIN を自己ホスト型 IIS の外部で独自のプロセス内の web アプリケーションに最適ですが、サーバーから web アプリケーションの分離 – Azure ワーカー ロール内などです。
> 
> このチュートリアルでは、Microsoft Azure のワーカー ロール内の OWIN アプリケーションを自己ホストする方法を学習します。 ワーカー ロールの詳細については、次を参照してください。 [Azure 実行モデル](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK for .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure プロジェクトを作成します。

管理者特権で Visual Studio を起動します。 Azure コンピューティング エミュレーターを使用してローカルで、アプリケーションをデバッグするには、管理者特権が必要です。

**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。 **インストールされたテンプレート**、Visual c# をクリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**です。 プロジェクト"AzureApp"の名前し、をクリックして**OK**です。

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

**新しい Windows Azure のクラウド サービス**ダイアログ ボックスをダブルクリックして**ワーカー ロール**です。 既定の名前 ("WorkerRole1") のままにします。 この手順は、ワーカー ロールをソリューションに追加します。 **[OK]** をクリックします。

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

Visual Studio ソリューション作成するにはには、2 つのプロジェクトが含まれています。

- &quot;AzureApp&quot;ロールと Azure アプリケーションの構成を定義します。
- &quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。

一般に、Azure アプリケーションは、このチュートリアルは、1 つの役割を使用していますが、複数のロールを含めることができます。

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>OWIN 自己ホスト パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>HTTP エンドポイントを追加します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。 ロール ノードを展開し、WorkerRole1 を右クリックし **プロパティ**です。

![](host-owin-in-an-azure-worker-role/_static/image6.png)

をクリックして**エンドポイント**、クリックして**エンドポイントの追加**です。

**プロトコル**ドロップダウン リストで、"http"を選択します。 **パブリック ポート**と**プライベート ポート**80」と入力します。 これらのポート番号は別にすることはできます。 パブリック ポートが、クライアントがロールに、要求を送信する際に使用します。

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>OWIN 起動クラスを作成します。

ソリューション エクスプ ローラーで、WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

次のようにすべての定型コードに置き換えます。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage`拡張メソッドが、サイトの動作を確認する、アプリケーションに、単純な HTML ページを追加します。

## <a name="start-the-owin-host"></a>OWIN ホストを起動します。

WorkerRole.cs ファイルを開きます。 このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。

次の追加ステートメントを使用します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

追加、 **IDisposable**メンバーを`WorkerRole`クラス。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

`OnStart`メソッド、ホストを起動する次のコードを追加します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start**メソッドは、OWIN ホストを開始します。 名前、`Startup`クラスは、型パラメーター、メソッドにします。 慣例により、ホストを呼び出すが、`Configure`このクラスのメソッドです。

上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

WorkerRole.cs の完全なコードを次に示します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

ソリューションをビルドし、Azure コンピューティング エミュレーターでアプリケーションをローカルで実行する場合は F5 キーを押します。 ファイアウォールの設定によっては、エミュレーター、ファイアウォールを通過を許可する必要があります。

コンピューティング エミュレーターは、エンドポイントに、ローカル IP アドレスを割り当てます。 コンピューティング エミュレーター UI を表示することによって IP アドレスを見つけることができます。 タスク バー通知領域に表示されたエミュレーター アイコンを右クリックし  **コンピューティング エミュレーター UI**です。

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

サービスの展開、展開 [id]、サービスの詳細情報の下の IP アドレスを検索します。 Web ブラウザーを開き、http:// に移動<em>アドレス</em>ここで、<em>アドレス</em>;、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、`http://127.0.0.1:80`です。 OWIN へようこそ ページを参照する必要があります。

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Azure への配置します。

この手順では、Azure アカウントが必要です。 既に持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)です。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。 **[発行]** を選びます。

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Azure アカウントにサインインしていない場合はクリックして**サイン イン**です。

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

サブスクリプションを選択しをクリックするサインイン後、**次**です。

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

クラウド サービスの名前を入力し、地域を選択します。 **[作成]** をクリックします。

![](host-owin-in-an-azure-worker-role/_static/image17.png)

**[発行]** をクリックします。

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Azure のアクティビティ ログ ウィンドウでは、展開の進行状況を示します。 アプリが展開されると、参照`http://appname.cloudapp.net/`ここで、 *appname*クラウド サービスの名前を指定します。

## <a name="additional-resources"></a>その他のリソース

- [プロジェクト Katana の概要](an-overview-of-project-katana.md)
- [GitHub の Katana プロジェクト](https://github.com/aspnet/AspNetKatana/)
