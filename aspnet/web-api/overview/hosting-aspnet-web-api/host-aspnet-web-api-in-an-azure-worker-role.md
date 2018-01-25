---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: "ASP.NET Web API 2 の Azure ワーカー ロールをホスト |Microsoft ドキュメント"
author: MikeWasson
description: "このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) de 用 Web インターフェイスを開く."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 9a7f8242bf482e81513accfe05e10a64ae0ca0b2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>ASP.NET Web API 2 の Azure ワーカー ロールをホストします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開く](http://owin.org/)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。 OWIN OWIN を自己ホスト型 IIS の外部で独自のプロセス内の web アプリケーションに最適ですが、サーバーから web アプリケーションの分離 – Azure ワーカー ロール内などです。
> 
> このチュートリアルではあります Microsoft.Owin.Host.HttpListener パッケージを使用して自己 OWIN アプリケーションをホストするために使用する HTTP サーバーを提供します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [Azure SDK for .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure プロジェクトを作成します。

管理者特権で Visual Studio を起動します。 Azure コンピューティング エミュレーターを使用してローカルで、アプリケーションをデバッグするには、管理者特権が必要です。

**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。 **インストールされたテンプレート**、Visual c# をクリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**です。 プロジェクト"AzureApp"の名前し、をクリックして**OK**です。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

**新しい Windows Azure のクラウド サービス**ダイアログ ボックスをダブルクリックして**ワーカー ロール**です。 既定の名前 ("WorkerRole1") のままにします。 この手順は、ワーカー ロールをソリューションに追加します。 **[OK]**をクリックします。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

Visual Studio ソリューション作成するにはには、2 つのプロジェクトが含まれています。

- &quot;AzureApp&quot;ロールと Azure アプリケーションの構成を定義します。
- &quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。

一般に、Azure アプリケーションは、このチュートリアルは、1 つの役割を使用していますが、複数のロールを含めることができます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API と OWIN パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>HTTP エンドポイントを追加します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。 ロール ノードを展開し、WorkerRole1 を右クリックし **プロパティ**です。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

をクリックして**エンドポイント**、クリックして**エンドポイントの追加**です。

**プロトコル**ドロップダウン リストで、"http"を選択します。 **パブリック ポート**と**プライベート ポート**80」と入力します。 これらのポート番号は別にすることはできます。 パブリック ポートが、クライアントがロールに、要求を送信する際に使用します。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Web API を構成する自己ホスト

ソリューション エクスプ ローラーで、WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

次のようにすべてのこのファイルに定型コードに置き換えます。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

次に、Web API コント ローラー クラスを追加します。 WorkerRole1 プロジェクトを右クリックし **追加** / **クラス**です。 TestController クラスの名前を付けます。 次のようにすべてのこのファイルに定型コードに置き換えます。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

わかりやすくするため、このコント ローラーにはプレーン テキストを返す 2 つの GET メソッドだけを定義します。

## <a name="start-the-owin-host"></a>OWIN ホストを起動します。

WorkerRole.cs ファイルを開きます。 このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。

次の追加ステートメントを使用します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

追加、 **IDisposable**メンバーを`WorkerRole`クラス。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

`OnStart`メソッド、ホストを起動する次のコードを追加します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start**メソッドは、OWIN ホストを開始します。 名前、`Startup`クラスは、型パラメーター、メソッドにします。 慣例により、ホストを呼び出すが、`Configure`このクラスのメソッドです。

上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

WorkerRole.cs の完全なコードを次に示します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

ソリューションをビルドし、Azure コンピューティング エミュレーターでアプリケーションをローカルで実行する場合は F5 キーを押します。 ファイアウォールの設定によっては、エミュレーター、ファイアウォールを通過を許可する必要があります。

> [!NOTE]
> 次のような例外が発生した場合を参照してください[このブログの投稿](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)問題を回避します。 "ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Owin, Version = 2.0.2.0、カルチャ = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つです。 見つかったアセンブリのマニフェスト定義は、アセンブリ参照と一致しません。 (HRESULT からの例外: 0x80131040)"


コンピューティング エミュレーターは、エンドポイントに、ローカル IP アドレスを割り当てます。 コンピューティング エミュレーター UI を表示することによって IP アドレスを見つけることができます。 タスク バー通知領域に表示されたエミュレーター アイコンを右クリックし  **コンピューティング エミュレーター UI**です。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

サービスの展開、展開 [id]、サービスの詳細情報の下の IP アドレスを検索します。 Web ブラウザーを開き、http:// に移動*アドレス*/テスト/1、ここで*アドレス*;、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、`http://127.0.0.1:80/test/1`です。 Web API コント ローラーからの応答が表示されます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Azure への配置します。

この手順では、Azure アカウントが必要です。 既に持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、「 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)です。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。 **[発行]** を選びます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Azure アカウントにサインインしていない場合はクリックして**サイン イン**です。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

サブスクリプションを選択しをクリックするサインイン後、**次**です。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

クラウド サービスの名前を入力し、地域を選択します。 **[作成]**をクリックします。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

**[発行]**をクリックします。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Azure のアクティビティ ログ ウィンドウでは、展開の進行状況を示します。 アプリが展開されると、http://appname.cloudapp.net/test/1 を参照します。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>その他のリソース

- [プロジェクト Katana の概要](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [GitHub の Katana プロジェクト](https://github.com/aspnet/AspNetKatana)
