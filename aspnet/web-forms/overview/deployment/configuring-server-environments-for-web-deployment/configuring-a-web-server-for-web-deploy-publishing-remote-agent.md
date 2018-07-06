---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Web サーバーを構成する web 配置発行 (リモート エージェント) |Microsoft Docs
author: jrjlee
description: このトピックでは、web の発行および IIS の Web デプロイを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 348c618fe6ec726e8087b4f3acb66cddbef1d225
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819644"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Web 配置発行 (リモート エージェント) の Web サーバーを構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web の発行および (Web 配置) IIS Web 配置ツールのリモート エージェント サービスを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法を説明します。
> 
> Web Deploy 2.0 以降を操作するときに、アプリケーションまたは web サーバーにサイトを取得する際、3 つの主な方法があります。 次の操作を行うことができます。
> 
> - 使用して、 *Web デプロイ エージェントのリモート サービス*します。 このアプローチには、web サーバーの以下の構成が必要ですが、サーバーにデプロイするためにローカル サーバーの管理者の資格情報を提供する必要があります。
> - 使用して、 *Web 配置ハンドラー*します。 このアプローチは、もっと複雑な web サーバーを設定する初期の労力が必要です。 ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。 Web 配置ハンドラーでは、IIS 7 以降のバージョンで利用できるのみです。
> - 使用*オフライン展開*します。 このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、サーバー上に web パッケージのコピーし IIS マネージャーからインポートする必要があります手動でします。
> 
> 主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>Web では、リモート エージェントの適切なアプローチを展開するのでしょうか。

はい、コンテンツを展開したユーザーは、移行先サーバーの管理者の資格情報を指定できます。 この方法は、これらの種類のシナリオでは望ましくは多くの場合です。

- 移行先の web サーバーとデータベース サーバーに対するフル コントロールを持つ開発者の開発またはテスト環境
- 小規模な組織で 1 人のユーザーまたはユーザーの小規模なグループが、全体のアプリケーション ライフ サイクルを制御します。

多くの大規模な組織とステージング環境または運用環境に特にで多くの場合は現実的では web サーバーで管理者権限をユーザーに付与します。 ホストされた web サーバーの場合でない場合に特に起こりやすくなります。 さらに、ビルド サーバーからのデプロイを自動化することを計画している、ない場合、展開プロセスに管理者の資格情報を使用します。 これらのシナリオで使用した展開をサポートするために web サーバーの構成、 [Web 配置ハンドラー](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)より満足できる選択肢を提供することがあります。

## <a name="task-overview"></a>タスクの概要

このトピックでは、インターネット インフォメーション サービス (IIS) 7.5 web サーバーをそのまま使用し、Web デプロイのリモート エージェントのアプローチを使用してリモート コンピューターから web パッケージの配置を構成する方法を説明します。 必要があります。

- IIS 7.5、および推奨構成は、IIS 7 をインストールします。
- Web Deploy 2.1 以降をインストールします。
- 展開されたコンテンツをホストする IIS の web サイトを作成します。
- Web 配置エージェント サービスが実行されていることを確認します。

具体的には、サンプル ソリューションをホストにもする必要があります。

- .NET Framework 4.0 をインストールします。
- ASP.NET MVC 3 をインストールします。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。 続行する前にいることを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーは、ドメインに参加しています。
- サーバーは、静的 IP アドレスを持ちます。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。 静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。 リモート エージェント サービスは、IIS 6 で以降はサポートされするドメインに参加させる必要はありません。 ただし、このチュートリアルの手順では開発され、IIS 7.5 でテストし、他のバージョンの手順が異なる場合があります。


## <a name="install-products-and-components"></a>製品とコンポーネントをインストールします。

このセクションで、web サーバーで必要な製品とコンポーネントのインストールを説明します。 開始する前に、サーバーが完全に最新であることを確認する Windows 更新プログラムを実行することをお勧めは。

この場合、これらをインストールする必要があります。

- **IIS 7 推奨構成**します。 これにより、 **Web サーバー (IIS)** web サーバー上のロールと IIS モジュールと ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。
- **.NET framework 4.0**します。 これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **Web 配置ツール 2.1 以降**します。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環として、インストールされ、Web Deployment Agent サービスを開始します。 このサービスでは、リモート コンピューターから web パッケージを配置することができます。
- **ASP.NET MVC 3**します。 MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。

> [!NOTE]
> このチュートリアルでは、Web Platform Installer をインストールに必要なコンポーネントの構成の使用について説明します。 Web Platform Installer を使用する必要はありません、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。 詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)します。


**必要な製品とコンポーネントをインストールするには**

1. ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。
2. インストールが完了したら、Web Platform Installer は自動的に起動されます。

    > [!NOTE]
    > いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。 これを実行する、**開始** メニューのをクリックして**すべてのプログラム**、順にクリックします**Microsoft Web Platform Installer**します。
3. 上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**します。
4. ナビゲーション ウィンドウで、ウィンドウの左側にある クリックして**フレームワーク**します。
5. **Microsoft .NET Framework 4** 、.NET Framework が既にインストールされていない場合、行をクリックして**追加**します。

    > [!NOTE]
    > 既にインストールしている Windows Update を通じて、.NET Framework 4.0。 製品またはコンポーネントが既にインストールされている場合、Web Platform Installer 示されます置き換えることで、**追加**ボタン テキストを含む**インストール済み**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. **ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。
7. ナビゲーション ウィンドウで、 **Server**します。
8. **IIS 7 推奨構成**行で、をクリックして**追加**します。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**します。
10. **[インストール]** をクリックします。 Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。
12. インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。

IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS に ASP.NET の最新バージョンを登録します。 これを行わないと、見つかります IIS が静的なコンテンツを提供 (HTML ファイル) など、問題なくが返されます**HTTP エラー 404.0 – 見つかりません**しようとすると、ASP.NET のコンテンツを参照します。 この手順を使用すると、ASP.NET 4.0 が登録されていることを確認します。

**ASP.NET 4.0 を IIS に登録するには**

1. クリックして**開始**、し、入力**コマンド プロンプト**します。
2. 右クリックし、検索結果で**コマンド プロンプト**、 をクリックし、**管理者として実行**します。
3. コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. 任意の時点の 64 ビット web アプリケーションをホストする場合は、IIS と ASP.NET の 64 ビット バージョンを登録することも必要があります。 コマンド プロンプト ウィンドウに移動します。、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。
6. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

お勧めとして Windows Update を使用してもう一度この時点でダウンロードして、新製品とコンポーネントをインストールしたら、使用可能な更新プログラムをインストールします。

## <a name="configure-the-iis-website"></a>IIS web サイトを構成します。

Web コンテンツを展開するには、サーバーに、前に作成してコンテンツをホストする IIS の web サイトを構成する必要があります。 Web Deploy; 既存の IIS web サイトに web パッケージを配置できるだけweb サイトを作成できません。 大まかに言えば、これらのタスクを完了する必要があります。

- コンテンツをホストするファイル システム上のフォルダーを作成します。
- コンテンツを処理するために IIS の web サイトを作成し、ローカル フォルダーに関連付けます。
- 読み取りローカル フォルダーにアプリケーション プール id へのアクセス許可を付与します。

IIS の既定の web サイトにコンテンツを展開するなんらですが、この方法はテストまたはデモ シナリオ以外は推奨されません。 運用環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。

**作成および IIS の web サイトを構成するには**

1. ローカル ファイル システムでコンテンツを格納するフォルダーを作成します (たとえば、 **C:\DemoSite**)。
2. **開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。
3. IIS マネージャーで、**接続**ウィンドウで、サーバー ノードを展開 (たとえば、 **TESTWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. 右クリックし、**サイト**ノード、およびクリック**Web サイトの追加**します。
5. **サイト名**ボックスに、IIS web サイトの名前を入力 (たとえば、 **DemoSite**)。
6. **物理パス**ボックス、入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。
7. **ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。

    > [!NOTE]
    > 標準ポート番号は、http 80 および HTTPS の場合は 443 です。 ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。
8. ままに、**ホスト名**ボックスをクリックして、web サイト用のドメイン ネーム システム (DNS) レコードを構成する場合、空白**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > 運用環境では、ポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成するします可能性があります。 IIS 7 でホスト ヘッダーの構成の詳細については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)します。 Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)します。
9. **[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。
10. **サイト バインド**ダイアログ ボックスで、をクリックして**追加**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. **サイト バインドの追加**ダイアログ ボックスで、セット、 **IP アドレス**と**ポート**既存のサイトの構成に一致するようにします。
12. **ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **TESTWEB1**)、順にクリックします**OK**。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > 最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`します。 2 つ目のサイトのバインドでは、マシン名を使用して、ドメインの他のコンピューターからサイトにアクセスできます (たとえば、http://testweb1:85)します。
13. **サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**します。
14. **接続**ウィンドウで、をクリックして**アプリケーション プール**します。
15. **アプリケーション プール**ウィンドウは、アプリケーション プールの名前を右クリックし、クリックして**基本設定**します。 既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。
16. **.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリックします**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > サンプル ソリューションでは、.NET Framework 4.0 が必要です。 これではなく Web デプロイのための要件です。

Web サイト コンテンツを配信するためには、アプリケーション プール id をコンテンツを格納するローカル フォルダーのアクセス許可読み取りが必要です。 IIS 7.5 では、アプリケーション プールは、(アプリケーション プールが通常実行されるネットワーク サービス アカウントを使用して、IIS の以前のバージョン) とは異なり、既定で一意のアプリケーション プール id で実行します。 アプリケーション プール id の実際のユーザー アカウントではないとユーザーまたはグループのすべてのリストが表示されない&#x2014;代わりにときに、作成されます動的にアプリケーション プールを開始します。 各アプリケーション プール id をローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示の項目として。

ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。

- アクセス許可を割り当てる、アプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).
- アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。

最も一般的なアプローチは、ローカルに権限を割り当てる、 **IIS\_IUSRS**のため、このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更できます。 グループ化します。 次の手順では、このグループ ベースのアプローチを使用します。

> [!NOTE]
> IIS 7.5 でアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。


**IIS の web サイトのフォルダーのアクセス許可を構成するには**

1. Windows エクスプ ローラーでは、ローカル フォルダーの場所を参照します。
2. フォルダーを右クリックし、**プロパティ**します。
3. **セキュリティ**] タブで [**編集**、順にクリックします**追加**します。
4. クリックして**場所**します。 **場所** ダイアログ ボックスでは、ローカルのサーバーを選択し、順にクリックします**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. **[ユーザーまたはグループ**ダイアログ ボックスに「 **IIS\_IUSRS**、] をクリックして**名前の確認**順にクリックします**OK**します。
6. <strong>のアクセス許可</strong><em>[フォルダー名]</em>ダイアログ ボックスで、新しいグループが割り当てられている通知、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。 変更なしのままにし、クリックして<strong>OK</strong>します。
7. をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。

最後のタスクとして、サーバーにすべての web パッケージをデプロイする前に行う必要があります、Web Deployment Agent サービスが実行されていること。 リモート コンピューターからパッケージを展開するときに、Web Deployment Agent サービスは抽出およびパッケージの内容をインストールする責任を負います。 サービスは、Web 配置ツールをインストールするときに、既定で開始され、ネットワーク サービスの id で実行されます。

確認できます、複数の異なる方法で、サービスが実行されているかどうかさまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用します。 この手順では、簡単な UI ベースのアプローチについて説明します。

**Web 配置エージェント サービスが実行されていることを確認するには**

1. **[スタート]** メニューで、 **[管理ツール]** をポイントして、 **[サービス]** をクリックします。
2. 検索、 **Web Deployment Agent サービス**行、および、いることを確認、**状態**に設定されている**開始**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. サービスがまだ開始されていない場合はクリックして**開始**します。

## <a name="configure-firewall-exceptions"></a>ファイアウォールの例外を構成します。

既定では、リモート エージェント サービスは この URL で TCP ポート 80 でリッスンします。

<http://servername.com/MSDEPLOYAGENTSERVICE>

ほとんどの場合、web サーバーは、通常ポート 80 で HTTP 要求をリッスンするため、リモート エージェント サービスの追加のファイアウォール規則を構成する必要はありません。 非標準ポートでリッスンするように、インストールをカスタマイズする場合は、必要に応じてファイアウォールの例外を構成する必要があります。

## <a name="conclusion"></a>まとめ

この時点で、web サーバーがそのまま使用してリモート コンピューターから web パッケージをインストールする準備ができてです。 サーバーに web アプリケーションをデプロイする前に、次の点を確認したい場合があります。

- IIS で ASP.NET 4.0 を登録したでしょうか。
- アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスをあるでしょうか。
- Web 配置エージェント サービスが実行されているか。

## <a name="further-reading"></a>関連項目

リモート エージェント サービスに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)します。

> [!div class="step-by-step"]
> [前へ](scenario-configuring-a-production-environment-for-web-deployment.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
