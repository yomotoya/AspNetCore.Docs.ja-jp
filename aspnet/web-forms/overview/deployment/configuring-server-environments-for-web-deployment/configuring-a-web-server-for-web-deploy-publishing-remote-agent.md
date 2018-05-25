---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Web サーバーを構成する web デプロイの発行 (リモート エージェント) |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、web の発行および IIS Web 配置を使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 9f3a55c5e68e61a2d7907c765209d3786e05a485
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Web デプロイの発行 (リモート エージェント) 用の Web サーバーの構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web の発行および IIS Web 配置ツール (Web 配置) のリモート エージェント サービスを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明します。
> 
> Web Deploy 2.0 以降を使用するときは、3 つの主な方法がアプリケーションまたは web サーバー上にサイトを取得するを使用することができます。 次の操作を行うことができます。
> 
> - 使用して、 *Web デプロイ エージェントのリモート サービス*です。 このアプローチには、web サーバーの以下の構成が必要ですが、何もサーバーに配置するためにローカル サーバーの管理者の資格情報を提供する必要があります。
> - 使用して、 *Web Deploy ハンドラー*です。 このアプローチはもっと複雑であり、web サーバーを設定する初期多くの労力が必要です。 ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。 Web 展開ハンドラーは IIS 7 以降のバージョンにできるだけです。
> - 使用して*オフライン展開*です。 このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、する必要があります手動でサーバーに web パッケージをコピーして IIS マネージャーからインポートします。
> 
> 主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>Web では、リモート エージェントの適切なアプローチを配置するのですか。

はい (コンテンツを展開したユーザーは、移行先サーバーの管理者の資格情報を提供できる場合)。 この方法は、これらの種類のシナリオでは望ましく、多くの場合。

- ターゲット web サーバーおよびデータベース サーバーに対するフル コントロールを持つ開発者の開発またはテスト環境
- 小規模な組織が 1 人のユーザーまたはユーザーの小さなグループに、全体のアプリケーション ライフ サイクルを制御します。

多くの大規模な組織とステージング環境または実稼働環境に特に、web サーバー上の管理者権限をユーザーに与える実際は、ほとんどの場合です。 ホストされる web サーバーの場合これは特に、大文字である可能性があります。 さらに、ビルド サーバーから展開を自動化する場合は、可能性がありますいないする展開プロセスに管理者の資格情報を使用します。 これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web 展開ハンドラー](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)さらに快適な選択肢を提供する場合があります。

## <a name="task-overview"></a>タスクの概要

このトピックでは、そのまま使用し、Web デプロイのリモート エージェントのアプローチを使用してリモート コンピューターから web パッケージを配置するインターネット インフォメーション サービス (IIS) 7.5 web サーバーを構成する方法について説明します。 必要があります。

- IIS 7.5、および IIS 7 の推奨構成をインストールします。
- Web Deploy 2.1 以降をインストールします。
- 展開されたコンテンツをホストする IIS の web サイトを作成します。
- Web Deployment Agent サービスが実行されていることを確認します。

サンプル ソリューションを具体的にはホストにもする必要があります。

- .NET Framework 4.0 をインストールします。
- ASP.NET MVC 3 をインストールします。

このトピックでは、各手順を実行する方法を示します。 タスクとここでチュートリアルは、Windows Server 2008 R2 を実行するクリーン サーバー ビルドを開始していることを想定しています。 続行する前に以下のことを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーがドメインに参加します。
- サーバーは、静的 IP アドレスを持ちます。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)です。 静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)です。 リモート エージェント サービスは、以降は IIS 6 でサポートされてし、するドメインに参加させる必要はありません。 ただし、このチュートリアルの手順が開発され、IIS 7.5 でテストされた、他のバージョンのプロシージャが異なる場合があります。


## <a name="install-products-and-components"></a>製品とコンポーネントをインストールします。

このセクションでは、web サーバーに必要な製品とコンポーネントをインストールを説明します。 開始する前に、お勧め、サーバーが最新であることを確認する Windows Update を実行します。

この場合、これらをインストールする必要があります。

- **IIS 7 の推奨構成**です。 これにより、 **Web サーバー (IIS)** ロール、web サーバー上の IIS モジュールおよび ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。
- **.NET framework 4.0**です。 これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **Web 配置ツール 2.1 以降**です。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環としてがインストールされ、Web Deployment Agent サービスを開始します。 このサービスでは、リモート コンピューターから web パッケージを展開できます。
- **ASP.NET MVC 3**です。 これは、MVC 3 アプリケーションを実行する必要があるアセンブリをインストールします。

> [!NOTE]
> このチュートリアルでは、Web Platform Installer をインストールして、必要なコンポーネントの構成の使用について説明します。 Web Platform Installer を使用できますが、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。 詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)です。


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

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. **ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**です。
7. ナビゲーション ウィンドウで **サーバー**です。
8. **IIS 7 の推奨構成**行で、をクリックして**追加**です。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**です。
10. **[インストール]** をクリックします。 Web Platform Installer が製品の一覧を表示&#x2014;関連する依存関係のいずれかと共に&#x2014;をインストールして、ライセンス条項に同意するように求められます。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。
12. インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。

IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS と ASP.NET の最新バージョンを登録します。 これを行わないと、ことがわかります (HTML ファイル) などの静的なコンテンツを IIS が提供する、問題なくが返されます**HTTP エラー 404.0-Not Found** ASP.NET のコンテンツを参照しようとします。 この手順を使用すると、ASP.NET 4.0 が登録されていることを確認してください。

**ASP.NET 4.0 を IIS に登録するには**

1. をクリックして**開始**、し、入力**コマンド プロンプト**です。
2. 検索結果を右クリックして**コマンド プロンプト**、クリックして**管理者として実行**です。
3. コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. 任意の時点での 64 ビットの web アプリケーションをホストする場合は、IIS と 64 ビット バージョンの ASP.NET を登録することも必要があります。 コマンド プロンプト ウィンドウで、これに移動、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。
6. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

をお勧め Windows Update を使用してもう一度この時点でダウンロードして、新しい製品とコンポーネントがインストールされている使用可能な更新プログラムをインストールします。

## <a name="configure-the-iis-website"></a>IIS web サイトを構成します。

Web コンテンツを展開するには、サーバーに、前に作成し、コンテンツをホストする IIS の web サイトを構成する必要があります。 Web Deploy にしかデプロイできません web パッケージ既存の IIS web サイトです。web サイトを作成できません。 大まかに言えば、これらのタスクを完了する必要があります。

- コンテンツをホストするファイル システムにフォルダーを作成します。
- コンテンツを提供する IIS の web サイトを作成し、ローカル フォルダーに関連付けます。
- 読み取り、アプリケーション プール id に、ローカル フォルダーに対する権限を付与します。

阻止する IIS の既定の web サイトにコンテンツを展開するからですが、この方法はテストまたはデモのシナリオ以外の何かの推奨されません。 実稼働環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。

**作成し、IIS の web サイトを構成します。**

1. ローカル ファイル システムにコンテンツを保存するフォルダーを作成します (たとえば、 **C:\DemoSite**)。
2. **開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。
3. IIS マネージャーで、**接続** ウィンドウで、サーバー ノードを展開 (たとえば、 **TESTWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. 右クリックし、**サイト**ノードをクリックして**Web サイトの追加**です。
5. **サイト名**ボックスで、IIS の web サイトの名前を入力 (たとえば、 **DemoSite**)。
6. **物理パス**ボックスに入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。
7. **ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。

    > [!NOTE]
    > 標準のポート番号は http が 80 および HTTPS の場合は 443 です。 ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。
8. ままにして、**ホスト名**ボックスをクリックして、web サイトのドメイン ネーム システム (DNS) レコードを構成するのでない限り、空白**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > 実稼働環境にすることができますをポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成します。 IIS 7 でのホスト ヘッダーを構成する方法については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)です。 Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)です。
9. **[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。
10. **サイト バインド**ダイアログ ボックスで、をクリックして**追加**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. **サイト バインドの追加**ダイアログ ボックスで、設定、 **IP アドレス**と**ポート**既存サイトの構成に一致するようにします。
12. **ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **TESTWEB1**)、をクリックし、 **[ok]** です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > 最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`です。 2 つ目のサイト バインドでは、コンピューター名を使用して、ドメインの他のコンピューターからサイトにアクセスすることができます (たとえば、http://testweb1:85)です。
13. **サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**です。
14. **接続** ウィンドウで、をクリックして**アプリケーション プール**です。
15. **アプリケーション プール** ウィンドウでは、アプリケーション プールの名前を右クリックし、をクリックして**基本設定**です。 既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。
16. **.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリック**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > サンプル ソリューションには、.NET Framework 4.0 が必要です。 これは、要件ではありません、Web Deploy の一般にします。

Web サイト コンテンツを提供するためには、アプリケーション プール id 読み取り権限が必要、コンテンツを格納するローカル フォルダーにします。 IIS 7.5、アプリケーション プールは、(ここでアプリケーション プールは通常の実行、Network Service アカウントを使用して、IIS の以前のバージョン) とは異なり、既定では、一意のアプリケーション プール id で実行します。 アプリケーション プール id が実際のユーザー アカウントではありませんし、ユーザーまたはグループのリストに表示されない&#x2014;代わりに、それが動的に作成、アプリケーション プールが開始されたときにします。 各アプリケーション プール id がローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示のアイテムとして。

ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。

- アクセス許可を割り当てるアプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).
- アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。

最も一般的な方法は、ローカルに権限を割り当てる、 **IIS\_IUSRS**このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更することができますので、グループ化します。 次の手順では、このグループ ベースのアプローチを使用します。

> [!NOTE]
> IIS 7.5 におけるアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。


**IIS の web サイトのフォルダーのアクセス許可を構成するには**

1. Windows エクスプ ローラーで、ローカル フォルダーの場所を参照します。
2. フォルダーを右クリックし、をクリックして**プロパティ**です。
3. **セキュリティ** タブで、をクリックして**編集**、クリックして**追加**です。
4. をクリックして**場所**です。 **場所**ダイアログ ボックスでは、ローカル サーバーを選択し、をクリックして**OK**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. **ユーザーまたはグループ** ダイアログ ボックスで、「 **IIS\_IUSRS**、 をクリックして**名前の確認**順にクリック**OK**です。
6. <strong>のアクセス許可</strong><em>[フォルダー名]</em> ダイアログ ボックスで、新しいグループが割り当てられている、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。 これを変更せずのままにし、をクリックして<strong>OK</strong>です。
7. をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。

最後のタスクとして、サーバーにすべての web パッケージを展開する前に行う必要があります Web Deployment Agent サービスが実行されていること。 リモート コンピューターからパッケージを展開するときに Web Deployment Agent サービスが、抽出およびパッケージの内容をインストールします。 サービスは、Web 配置ツールをインストールするときに、既定で開始され、Network Service の id で実行されます。

確認できます、複数のさまざまな方法でサービスが実行されているかどうかさまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用します。 この手順では、UI ベースの簡単な方法について説明します。

**Web Deployment Agent サービスが実行されていることを確認するには**

1. **[スタート]** メニューで、 **[管理ツール]** をポイントして、 **[サービス]** をクリックします。
2. 検索、 **Web Deployment Agent サービス**行をしていることを確認、**ステータス**に設定されている**Started**です。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. サービスが開始されていない場合はクリックして**開始**です。

## <a name="configure-firewall-exceptions"></a>ファイアウォールの例外を構成します。

既定では、リモート エージェント サービスはこの URL で、TCP ポート 80 でリッスンします。

<http://servername.com/MSDEPLOYAGENTSERVICE>

ほとんどの場合、web サーバーが通常リッスン ポート 80 で HTTP 要求のために、リモート エージェント サービスの追加のファイアウォール ルールを構成する必要はありません。 非標準ポートでリッスンするように、インストールをカスタマイズする場合は、必要に応じてファイアウォールの例外を構成する必要があります。

## <a name="conclusion"></a>まとめ

この時点では、web サーバーをそのまま使用して、リモート コンピューターから web パッケージをインストールする準備ができてです。 Web アプリケーション サーバーを展開する前に、これらの要点をチェックすることがあります。

- IIS で ASP.NET 4.0 を登録したしますか。
- アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスがしますか。
- Web Deployment Agent サービスが実行されているか。

## <a name="further-reading"></a>関連項目

リモート エージェント サービスに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)です。

> [!div class="step-by-step"]
> [前へ](scenario-configuring-a-production-environment-for-web-deployment.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
