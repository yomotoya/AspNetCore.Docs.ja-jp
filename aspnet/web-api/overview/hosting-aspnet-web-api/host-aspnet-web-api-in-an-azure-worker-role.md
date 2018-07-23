---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: ASP.NET Web API 2 では、Azure Worker ロールのホスト |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) de の Web インターフェイスを開く.
ms.author: aspnetcontent
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: c53256b8a72a377f51b9fbac7944657cb6d4c6e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803851"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>ASP.NET Web API 2 では、Azure Worker ロールをホストします。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストする Azure ワーカー ロールで ASP.NET Web API をホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開き](http://owin.org/)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションの分離 – Azure worker ロール内など。
> 
> このチュートリアルでは、Microsoft.Owin.Host.HttpListener パッケージを使用します OWIN アプリケーションを自己ホストするために使用する HTTP サーバーを提供します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - [Azure SDK for .NET 2.3](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a>Microsoft Azure プロジェクトを作成します。

管理者特権で Visual Studio を起動します。 Azure コンピューティング エミュレーターを使用してローカルでのアプリケーションをデバッグするには、管理者特権が必要です。

**ファイル** メニューのをクリックして**新規**、 をクリックし、**プロジェクト**します。 **インストールされたテンプレート**、Visual c# では、クリックして**クラウド** をクリックし、 **Windows Azure クラウド サービス**します。 プロジェクトとして「AzureApp」という名前にして**OK**します。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

**新しい Windows Azure クラウド サービス**ダイアログ ボックスで、ダブルクリックして**ワーカー ロール**します。 既定の名前 ("WorkerRole1") のままにします。 この手順では、ソリューションにワーカー ロールを追加します。 **[OK]** をクリックします。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

作成された Visual Studio ソリューションには、2 つのプロジェクトが含まれています。

- &quot;AzureApp&quot;ロールと Azure のアプリケーションの構成を定義します。
- &quot;WorkerRole1&quot;ワーカー ロールのコードが含まれています。

一般に、Azure のアプリケーションは、このチュートリアルは、1 つのロールを使用しますが、複数のロールを含めることができます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API と OWIN パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。

パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>HTTP エンドポイントを追加します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを展開します。 ロール ノードを展開し、WorkerRole1 を右クリックし、**プロパティ**します。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

クリックして**エンドポイント**、 をクリックし、**エンドポイントの追加**します。

**プロトコル**ドロップダウン リストで、"http"を選択します。 **パブリック ポート**と**プライベート ポート**80」と入力します。 これらのポート番号が異なることがあります。 パブリック ポートは、ロール要求を送信するときに使用するクライアントです。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>用の Web API を構成する自己ホスト

ソリューション エクスプ ローラーで WorkerRole1 プロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

このファイルの定型コードのすべて、次に置き換えます。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

次に、Web API コント ローラー クラスを追加します。 WorkerRole1 プロジェクトを右クリックして**追加** / **クラス**します。 TestController クラスの名前を付けます。 このファイルの定型コードのすべて、次に置き換えます。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

わかりやすくするため、このコント ローラーはプレーン テキストを返す 2 つの GET メソッドだけを定義します。

## <a name="start-the-owin-host"></a>OWIN ホストを開始します。

WorkerRole.cs ファイルを開きます。 このクラスは、ワーカー ロールが開始および停止時に実行されるコードを定義します。

次の追加ステートメントを使用します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

追加、 **IDisposable**メンバーを`WorkerRole`クラス。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

`OnStart`メソッドでは、ホストを起動する次のコードを追加します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

**WebApp.Start**メソッドは、OWIN ホストを開始します。 名前、`Startup`クラスは、メソッドの型パラメーター。 慣例により、ホストが呼び出す、`Configure`このクラスのメソッド。

上書き、`OnStop`を破棄する、 *\_アプリ*インスタンス。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

WorkerRole.cs の完全なコードを次に示します。

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

ソリューションをビルドし、f5 キーを押して Azure コンピューティング エミュレーターでアプリケーションをローカルで実行します。 ファイアウォールの設定によっては、ファイアウォール経由のエミュレーターを許可する必要があります。

> [!NOTE]
> 次のような例外が発生した場合を参照してください[このブログの投稿](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx)問題を回避します。 "ファイルまたはアセンブリを読み込むことができません ' Microsoft.Owin、バージョン 2.0.2.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つ。 指定したアセンブリのマニフェスト定義では、アセンブリ参照は一致しません。 (HRESULT からの例外: 0x80131040)"


コンピューティング エミュレーターでは、エンドポイントに、ローカル IP アドレスが割り当てられます。 コンピューティング エミュレーター UI を表示することによって、IP アドレスを見つけることができます。 タスク バー通知領域のエミュレーター アイコンを右クリックして**コンピューティング エミュレーター UI**します。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

サービスの展開で、サービスの詳細情報の展開 [id]、IP アドレスを検索します。 Web ブラウザーを開き、http:// に移動します<em>アドレス</em>/テスト/1、位置<em>アドレス</em>は、コンピューティング エミュレーターによって割り当てられた IP アドレスは、たとえば、 `http://127.0.0.1:80/test/1`。 Web API コント ローラーからの応答が表示されます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Azure に配置する

この手順では、Azure アカウントが必要です。 1 つをいない場合は、ほんの数分で無料試用版アカウントを作成できます。 詳細については、次を参照してください。 [Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。

ソリューション エクスプ ローラーで、AzureApp プロジェクトを右クリックします。 **[発行]** を選びます。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Azure アカウントにサインインしていない場合はクリックして**サインイン**します。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

サインインした後、サブスクリプションを選択およびクリックして**次**します。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

クラウド サービスの名前を入力し、リージョンを選択します。 **[作成]** をクリックします。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

**[発行]** をクリックします。

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

Azure アクティビティ ログ ウィンドウには、展開の進行状況が表示されます。 アプリが展開されると、参照 http://appname.cloudapp.net/test/1 します。

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>その他のリソース

- [プロジェクト Katana の概要](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [GitHub の Katana プロジェクト](https://github.com/aspnet/AspNetKatana)
