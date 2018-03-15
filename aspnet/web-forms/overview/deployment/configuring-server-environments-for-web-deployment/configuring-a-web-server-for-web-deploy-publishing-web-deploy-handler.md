---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: "デプロイの発行設定 Web 用 Web サーバーの構成 (Web Deploy ハンドラー) |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、web の発行および IIS Web 配置ハングルを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 81848c683fb9ddaa8942f030a520847a3c89fde0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>デプロイの発行設定 Web 用 Web サーバーの構成 (Web Deploy ハンドラー)
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web の発行と、IIS Web 配置ハンドラーを使用して展開をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明します。
> 
> Web Deploy 2.0 以降を使用するときは、3 つの主な方法がアプリケーションまたは web サーバー上にサイトを取得するを使用することができます。 次の操作を行うことができます。
> 
> - 使用して、 *Web デプロイ エージェントのリモート サービス*です。 このアプローチには、web サーバーの以下の構成が必要ですが、何もサーバーに配置するためにローカル サーバーの管理者の資格情報を提供する必要があります。
> - 使用して、 *Web Deploy ハンドラー*です。 このアプローチはもっと複雑であり、web サーバーを設定する初期多くの労力が必要です。 ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。 Web 展開ハンドラーは IIS 7 以降のバージョンにできるだけです。
> - 使用して*オフライン展開*です。 このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、する必要があります手動でサーバーに web パッケージをコピーして IIS マネージャーからインポートします。
> 
> 主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。


特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する場合ははい。 この方法は、これらの種類のシナリオでは望ましく、多くの場合。

- ステージング環境または運用環境、リモート展開をトリガーする人物またはサービス アカウントは、サーバー管理者の資格情報にアクセスする可能性があります。
- ホスト環境は、リモート ユーザーが web サーバー (または他のユーザーの web サイトへのアクセス) のフル コントロールを与えることがなく、web サイトを更新できるようにします。

開発またはテストのシナリオまたは小規模な組織では、サーバー管理者の資格情報を使用して展開するコンテンツが多くの場合、小さい悪化します。 これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web リモート エージェント サービスの展開](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)さらにわかりやすいアプローチを提供します。

## <a name="task-overview"></a>タスクの概要

Web 展開ハンドラー アプローチを使用してリモート コンピューターから web パッケージを配置して受け入れ、web サーバーを構成するのには、する必要があります。

- 作成、またはドメイン ユーザー アカウント (「管理者以外のユーザー」) の展開を実行に使用する資格情報を選択します。
- IIS 7.5、Web 管理サービスなど、基本的な認証モジュールをインストールします。
- Web Deploy 2.1 以降をインストールします。
- リモート接続を許可する Web 管理サービスを構成し、サービスを開始します。
- 展開されたコンテンツをホストする IIS の web サイトを作成します。
- IIS マネージャーで web サイトの管理者以外のユーザー アクセス許可を付与します。
- 委任規則を追加し、管理者以外のユーザー アカウントを使用して web サイトのコンテンツの変更をサービスに許可する Web 管理サービスを確認してください。
- ポート 8172 で着信接続を許可するファイアウォールを構成します。

ContactManager サンプル ソリューションを具体的にはホストにもする必要があります。

- .NET Framework 4.0 をインストールします。
- ASP.NET MVC 3 をインストールします。

このトピックでは、各手順を実行する方法を示します。 タスクとここでチュートリアルは、Windows Server 2008 R2 を実行するクリーン サーバー ビルドを開始していることを想定しています。 続行する前に以下のことを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーがドメインに参加します。
- サーバーは、静的 IP アドレスを持ちます。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)です。 静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)です。


## <a name="install-products-and-components"></a>製品とコンポーネントをインストールします。

このセクションでは、web サーバーに必要な製品とコンポーネントをインストールを説明します。 開始する前に、お勧め、サーバーが最新であることを確認する Windows Update を実行します。

この場合、これらをインストールする必要があります。

- **IIS 7 の推奨構成**です。 これにより、 **Web サーバー (IIS)**ロール、web サーバー上の IIS モジュールおよび ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。
- **IIS: 管理サービス**です。 これにより、IIS で Web 管理サービス (WMSvc) がインストールされます。 このサービスは、IIS web サイトのリモート管理を有効にし、クライアントに Web 配置ハンドラー エンドポイントを公開します。
- **IIS: 基本認証**です。 これには、基本認証の IIS モジュールがインストールされます。 これにより、Web 管理サービス (WMSvc) は、指定した資格情報を認証します。
- **Web 配置ツール 2.1 以降**です。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環として、Web 配置のハンドラーをインストールし、Web 管理サービスと統合します。
- **.NET Framework 4.0**. これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **ASP.NET MVC 3**. これは、MVC 3 アプリケーションを実行する必要があるアセンブリをインストールします。

> [!NOTE]
> このチュートリアルでは、Web Platform Installer をインストールして構成するさまざまなコンポーネントの使用について説明します。 Web Platform Installer を使用できますが、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。 詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)です。


**必要な製品とコンポーネントをインストールするには**

1. ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。
2. インストールが完了したら、Web Platform Installer が自動的に起動します。

    > [!NOTE]
    > いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。 [、**開始**] メニューのをクリックして**すべてのプログラム**、順にクリック**Microsoft Web Platform Installer**です。
3. 上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**です。
4. ナビゲーション ウィンドウで、ウィンドウの左側にあるをクリックして**フレームワーク**です。
5. **Microsoft .NET Framework 4** 、.NET Framework がインストールされていない場合、行をクリックして**追加**です。

    > [!NOTE]
    > 既にインストールしている Windows Update から .NET Framework 4.0。 製品またはコンポーネントがインストール済みの場合、Web Platform Installer は、これに置き換えることで、**追加**ボタン テキストを**インストール**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. **ASP.NET MVC 3 (Visual Studio 2010)**行で、をクリックして**追加**です。
7. ナビゲーション ウィンドウで **サーバー**です。
8. **IIS 7 の推奨構成**行で、をクリックして**追加**です。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**です。
10. **IIS: 基本認証**行で、をクリックして**追加**です。
11. **IIS: 管理サービス**行で、をクリックして**追加**です。
12. **[インストール]**をクリックします。 Web Platform Installer をインストールするには、関連する依存関係 & #x 2014; と共に; 製品 & #x 2014 の一覧が表示され、ライセンス条項に同意するように求められます。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。
14. インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。

IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS と ASP.NET の最新バージョンを登録します。 これを行わないと、ことがわかります (HTML ファイル) などの静的なコンテンツを IIS が提供する、問題なくが返されます**HTTP エラー 404.0-Not Found** ASP.NET のコンテンツを参照しようとします。 次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認してください。

**ASP.NET 4.0 を IIS に登録するには**

1. をクリックして**開始**、し、入力**コマンド プロンプト**です。
2. 検索結果を右クリックして**コマンド プロンプト**、クリックして**管理者として実行**です。
3. コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. 任意の時点での 64 ビットの web アプリケーションをホストする場合は、IIS と 64 ビット バージョンの ASP.NET を登録することも必要があります。 コマンド プロンプト ウィンドウで、これに移動、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。
6. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

をお勧め Windows Update を使用してもう一度この時点でダウンロードして、新しい製品とコンポーネントがインストールされている使用可能な更新プログラムをインストールします。

## <a name="configure-the-web-management-service"></a>Web 管理サービスを構成します。

必要なものをインストールしたら、次の手順は、IIS で Web 管理サービスを構成するは。 大まかに言えば、これらのタスクを完了する必要があります。

- サーバー レベルで基本認証を有効にします。
- リモート接続を許可する Web 管理サービスを構成します。
- Web 管理サービスを開始します。
- 必要な Web 管理サービス委任規則が満たされていることを確認します。

**Web 管理サービスを構成するには**

1. **開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。
2. IIS マネージャーで、**接続** ウィンドウで、サーバー ノードをクリックして (たとえば、 **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. 中央のウィンドウで  **IIS**をダブルクリックして**認証**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. 右クリック**基本認証**、クリックして**を有効にする**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. **接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。
6. 中央のウィンドウで **管理**をダブルクリックして**管理サービス**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. 中央のウィンドウで次のように選択します。**リモート接続を有効にする**です。

    > [!NOTE]
    > Web 管理サービスが既に実行されている場合は、最初に停止する必要があります。
8. **アクション** ウィンドウで、をクリックして**開始**Web 管理サービスを開始します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. 設定を保存するメッセージが表示されたら、クリックして**はい**です。

    > [!NOTE]
    > 自動的に開始するサービスを構成することもできます。 これを行うには、サービス コンソールを開きを右クリックして**Web 管理サービス**、クリックして**プロパティ**です。 **スタートアップの種類**ドロップダウン リストで、**自動**、順にクリック**OK**です。
10. **接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。
11. 中央のウィンドウで **管理**をダブルクリックして**管理サービス委任**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. 中央のペインに、一連ルールにはが含まれていることを確認します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    これらの規則は、さまざまな Web Deploy プロバイダーを使用する承認された Web 管理サービスのユーザーを許可します。 たとえば、web アプリケーションとコンテンツを Web 配置のハンドラーを使用して IIS を展開する必要がありますすべて使用する Web 管理サービスのユーザーを認証できるようにする委任規則、 **contentPath**と**iisApp**プロバイダー (、前回のルールのスクリーン ショットに表示される)。

    このトピックで説明した順序での製品とコンポーネントをインストールした場合、最新バージョンの Web Deploy 必要があります自動的に追加すべての必要な委任規則 Web 管理サービス。 管理サービスの委任のページで、規則が表示されない場合は、自分で作成する必要があります。 これを行う方法については、次を参照してください。 [Web の展開のハンドラーを構成する](https://go.microsoft.com/?linkid=9805124)です。
13. **接続** ウィンドウで、最上位レベルの設定に戻るには、もう一度サーバー ノードをクリックします。

## <a name="create-and-configure-an-iis-website"></a>作成し、IIS の web サイトを構成します。

Web コンテンツを展開するには、サーバーに、前に作成し、コンテンツをホストする IIS の web サイトを構成する必要があります。 Web Deploy にしかデプロイできません web パッケージ既存の IIS web サイトです。web サイトを作成できません。 また、非管理者アカウントは、コンテンツをリモートで展開を許可するほとんどの余分な構成を行う必要があります。 大まかに言えば、これらのタスクを完了する必要があります。

- コンテンツをホストするファイル システムにフォルダーを作成します。
- コンテンツを提供する IIS の web サイトを作成し、ローカル フォルダーに関連付けます。
- 読み取り、アプリケーション プール id に、ローカル フォルダーに対する権限を付与します。
- Web アプリケーションを展開するドメイン アカウントに必要な IIS のアクセス許可を付与します。

阻止する IIS の既定の web サイトにコンテンツを展開するからですが、この方法はテストまたはデモのシナリオ以外の何かの推奨されません。 実稼働環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。

**IIS の web サイトを作成するには**

1. ローカル ファイル システムにコンテンツを保存するフォルダーを作成します (たとえば、 **C:\DemoSite**)。
2. **開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。
3. IIS マネージャーで、**接続** ウィンドウで、サーバー ノードを展開 (たとえば、 **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. 右クリックし、**サイト**ノードをクリックして**Web サイトの追加**です。
5. **サイト名**ボックスで、IIS の web サイトの名前を入力 (たとえば、 **DemoSite**)。
6. **物理パス**ボックスに入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。
7. **ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。

    > [!NOTE]
    > 標準のポート番号は http が 80 および HTTPS の場合は 443 です。 ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。
8. ままにして、**ホスト名**ボックスをクリックして、web サイトのドメイン ネーム システム (DNS) レコードを構成するのでない限り、空白**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > 実稼働環境にすることができますをポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成します。 IIS 7 でのホスト ヘッダーを構成する方法については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)です。 Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)です。
9. **[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。
10. **サイト バインド**ダイアログ ボックスで、をクリックして**追加**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. **サイト バインドの追加**ダイアログ ボックスで、設定、 **IP アドレス**と**ポート**既存サイトの構成に一致するようにします。
12. **ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **STAGEWEB1**)、をクリックし、 **[ok]**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > 最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`です。 2 つ目のサイト バインドでは、コンピューター名 (たとえば、http://stageweb1:85) を使用して、ドメインの他のコンピューターからサイトにアクセスすることができます。
13. **サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**です。
14. **接続** ウィンドウで、をクリックして**アプリケーション プール**です。
15. **アプリケーション プール** ウィンドウでは、アプリケーション プールの名前を右クリックし、をクリックして**基本設定**です。 既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。
16. **.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリック**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > サンプル ソリューションには、.NET Framework 4.0 が必要です。 これは、要件ではありません、Web Deploy の一般にします。

Web サイト コンテンツを提供するためには、アプリケーション プール id 読み取り権限が必要、コンテンツを格納するローカル フォルダーにします。 IIS 7.5、アプリケーション プールは、(ここでアプリケーション プールは通常の実行、Network Service アカウントを使用して、IIS の以前のバージョン) とは異なり、既定では、一意のアプリケーション プール id で実行します。 アプリケーション プール id が実際のユーザー アカウントではないと、ユーザーまたはグループ & #x 2014 のすべてのリストに表示されないです。 代わりに、それが動的に作成、アプリケーション プールが開始されたときにします。 各アプリケーション プール id がローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示のアイテムとして。

ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。

- アクセス許可を割り当てるアプリケーション プール id に直接、形式を使用して**IIS AppPool\***[アプリケーション プール名] * (たとえば、 **IIS AppPool\DemoSite**)。
- アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。

最も一般的な方法は、ローカルに権限を割り当てる、 **IIS\_IUSRS**このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更することができますので、グループ化します。 次の手順では、このグループ ベースのアプローチを使用します。

> [!NOTE]
> IIS 7.5 におけるアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。


**IIS の web サイトのフォルダーのアクセス許可を構成するには**

1. Windows エクスプ ローラーで、ローカル フォルダーの場所を参照します。
2. フォルダーを右クリックし、をクリックして**プロパティ**です。
3. **セキュリティ** タブで、をクリックして**編集**、クリックして**追加**です。
4. をクリックして**場所**です。 **場所**ダイアログ ボックスでは、ローカル サーバーを選択し、をクリックして**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. **ユーザーまたはグループ** ダイアログ ボックスで、「 **IIS\_IUSRS**、 をクリックして**名前の確認**順にクリック**OK**です。
6. **のアクセス許可 * * * [フォルダー名]*  ダイアログ ボックスで、新しいグループが割り当てられている、**読み取り&amp;実行**、**フォルダー内容の一覧**、および**読み取り**既定のアクセスを許可します。 これを変更せずのままにし、をクリックして**OK**です。
7. をクリックして**OK**を閉じる、 *[フォルダー名] * * * プロパティ** ダイアログ ボックス。

最後のタスクとして、管理者以外の資格情報にユーザーのコンテンツを展開するときに使用する適切なアクセス許可を付与する必要があります。 このユーザーには、web サイトにコンテンツをリモートで展開する権限が必要です。

**管理者以外のドメイン ユーザーの IIS web サイトのアクセス許可を構成するには**

1. IIS マネージャーで、**接続** ウィンドウで、web サイト ノードを右クリックして (たとえば、 **DemoSite**)、 をポイント**展開**、クリックして**Web の構成デプロイの発行**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. **Web 配置をパブリッシング**の右側に、ダイアログ ボックス、**発行アクセス許可を付与するユーザーを選択**一覧で、省略記号ボタンをクリックします。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. **を許可するユーザー**  ダイアログ ボックスを使用して、コンテンツを展開して、をクリックするアカウントのドメインとユーザーの名前を入力**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. **Web 配置をパブリッシング**ダイアログ ボックスで、をクリックして**セットアップ**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > この操作は、1 つの手順で 2 つのキー機能を実行します。 最初に、前のセクションで確認する委任の規則に従って、Web 管理サービスを使用してリモートで web サイトを変更するユーザーの権限を付与します。 次に、追加、変更、および web サイトのコンテンツに対する権限を設定することができますが、web サイト用のソース フォルダーのユーザーのフル コントロールを付与します。
5. **Web 配置をパブリッシング**ダイアログ ボックスで、をクリックして**閉じる**です。

## <a name="configure-firewall-exceptions"></a>ファイアウォールの例外を構成します。

既定では、IIS Web 管理サービスは TCP ポート 8172 でリッスンします。 Web サーバーで Windows ファイアウォールが有効な場合は、ルールを作成する新しい受信ポート 8172 で TCP トラフィックを許可する (すべての送信トラフィックは既定では、Windows ファイアウォールで許可されている) 必要があります。 サードパーティ製のファイアウォールを使用する場合は、トラフィックを許可する規則を作成する必要があります。

| Direction | ポートから | ポートに | ポートの種類 |
| --- | --- | --- | --- |
| 受信 | どれでも可 | 8172 | TCP |
| 送信 | 8172 | どれでも可 | TCP |
  

Windows ファイアウォールのルールの構成の詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)です。 サードパーティ製のファイアウォール製品のマニュアルを参照してください。

## <a name="conclusion"></a>まとめ

Web サーバーは、Web 管理サービスを使用して、Web 配置ハンドラーへのリモート展開を受け入れる準備ができてができます。 Web アプリケーション サーバーを展開する前に、これらの要点をチェックすることがあります。

- IIS でサーバー レベルで基本認証を有効にしますか。
- Web 管理サービスへのリモート接続を有効にしますか。
- Web 管理サービスを開始しましたか。
- ルールは、ある管理サービス委任インプレース?
- アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスがしますか。
- 管理者以外のユーザー アカウントはサイト レベルのアクセス許可を IIS でですか。
- ファイアウォールは TCP ポート 8172 でサーバーへの着信接続を許可しますか。

## <a name="further-reading"></a>関連項目

Web 展開ハンドラーに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)です。

>[!div class="step-by-step"]
[前へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[次へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
