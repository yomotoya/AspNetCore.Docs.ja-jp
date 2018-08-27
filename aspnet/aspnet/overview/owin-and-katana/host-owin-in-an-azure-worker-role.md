---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Azure Worker ロールで OWIN ホスト |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、Microsoft Azure worker ロールで OWIN を自己ホストする方法を示します。 Open Web Interface for .NET (OWIN) は、.NET の web サーバー間の抽象化を定義しています.
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 6bead915491c62de809b8625d8071a63c70a6ef5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828895"
---
<a name="host-owin-in-an-azure-worker-role"></a>Azure Worker ロールで OWIN をホスト
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> このチュートリアルでは、Microsoft Azure worker ロールで OWIN を自己ホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開き](http://owin.org/)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションの分離 – Azure worker ロール内など。
> 
> このチュートリアルでは、Microsoft Azure worker ロール内での OWIN アプリケーションを自己ホストする方法を学習します。 ワーカー ロールの詳細については、次を参照してください。 [Azure 実行モデル](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices)します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK for .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure プロジェクトを作成します。

管理者特権で Visual Studio を起動します。 Azure コンピューティング エミュレーターを使用してローカルでのアプリケーションをデバッグするには、管理者特権が必要です。

**ファイル** メニューのをクリックして**新規**、 をクリックし、**プロジェクト**します。 **インストールされたテンプレート**、Visual c# では、クリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**します。 プロジェクトとして「AzureApp」という名前にして**OK**します。

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ダブルクリックして**ワーカー ロール**します。 既定の名前 ("WorkerRole1") のままにします。 この手順では、ソリューションにワーカー ロールを追加します。 **[OK]** をクリックします。

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

作成された Visual Studio ソリューションには、2 つのプロジェクトが含まれています。

- &quot;AzureApp&quot;ロールと Azure のアプリケーションの構成を定義します。
- &quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。

一般に、Azure のアプリケーションは、このチュートリアルは、1 つのロールを使用しますが、複数のロールを含めることができます。

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>OWIN 自己ホスト パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>HTTP エンドポイントを追加します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。 ロール ノードを展開し、WorkerRole1 を右クリックし、**プロパティ**します。

![](host-owin-in-an-azure-worker-role/_static/image6.png)

クリックして**エンドポイント**、 をクリックし、**エンドポイントの追加**します。

**プロトコル**ドロップダウン リストで、"http"を選択します。 **パブリック ポート**と**プライベート ポート**80」と入力します。 これらのポート番号が異なることがあります。 パブリック ポートは、ロール要求を送信するときに使用するクライアントです。

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>OWIN Startup クラスを作成します。

ソリューション エクスプ ローラーで WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

次のようにすべての定型コードに置き換えます。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

`UseWelcomePage`拡張メソッドは、アプリケーションには、サイトの動作を確認する単純な HTML ページを追加します。

## <a name="start-the-owin-host"></a>OWIN ホストを開始します。

WorkerRole.cs ファイルを開きます。 このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。

次の追加ステートメントを使用します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

追加、 **IDisposable**メンバーを`WorkerRole`クラス。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

`OnStart`メソッドでは、ホストを起動する次のコードを追加します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

**WebApp.Start**メソッドは、OWIN ホストを開始します。 名前、`Startup`クラスは、メソッドの型パラメーター。 慣例により、ホストが呼び出す、`Configure`このクラスのメソッド。

上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

WorkerRole.cs の完全なコードを次に示します。

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

ソリューションをビルドし、f5 キーを押して Azure コンピューティング エミュレーターでアプリケーションをローカルで実行します。 ファイアウォールの設定によっては、ファイアウォール経由のエミュレーターを許可する必要があります。

コンピューティング エミュレーターでは、エンドポイントに、ローカル IP アドレスが割り当てられます。 コンピューティング エミュレーター UI を表示することによって、IP アドレスを見つけることができます。 タスク バー通知領域のエミュレーター アイコンを右クリックして**コンピューティング エミュレーター UI**します。

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

サービスの展開で、サービスの詳細情報の展開 [id]、IP アドレスを検索します。 Web ブラウザーを開き、http:// に移動します<em>アドレス</em>ここで、<em>アドレス</em>は、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、`http://127.0.0.1:80`します。 OWIN へようこそ ページを参照する必要があります。

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Azure に配置する

この手順では、Azure アカウントが必要です。 1 つをいない場合は、ほんの数分で無料試用版アカウントを作成できます。 詳細については、次を参照してください。 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。 **[発行]** を選びます。

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Azure アカウントにサインインしていない場合はクリックして**サインイン**します。

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

サインインした後、サブスクリプションを選択およびクリックして**次**します。

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

クラウド サービスの名前を入力し、リージョンを選択します。 **[作成]** をクリックします。

![](host-owin-in-an-azure-worker-role/_static/image17.png)

**[発行]** をクリックします。

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

Azure アクティビティ ログ ウィンドウには、展開の進行状況が表示されます。 アプリが展開されると、参照`http://appname.cloudapp.net/`ここで、 *appname*クラウド サービスの名前を指定します。

## <a name="additional-resources"></a>その他のリソース

- [プロジェクト Katana の概要](an-overview-of-project-katana.md)
- [GitHub の Katana プロジェクト](https://github.com/aspnet/AspNetKatana/)
