---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Web サーバーを構成する web 配置発行 (Web 配置ハンドラー) |Microsoft Docs
author: jrjlee
description: このトピックでは、web の発行および IIS Web 配置 Han を使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 3296bb9b6460bbe80782746c9d398aa67815dcee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833336"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Web サーバーを構成する web 配置発行 (Web 配置ハンドラー)
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web の発行および IIS Web 配置ハンドラーを使用して配置をサポートするためにインターネット インフォメーション サービス (IIS) web サーバーを構成する方法を説明します。
> 
> Web Deploy 2.0 以降を操作するときに、アプリケーションまたは web サーバーにサイトを取得する際、3 つの主な方法があります。 次の操作を行うことができます。
> 
> - 使用して、 *Web デプロイ エージェントのリモート サービス*します。 このアプローチには、web サーバーの以下の構成が必要ですが、サーバーにデプロイするためにローカル サーバーの管理者の資格情報を提供する必要があります。
> - 使用して、 *Web 配置ハンドラー*します。 このアプローチは、もっと複雑な web サーバーを設定する初期の労力が必要です。 ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。 Web 配置ハンドラーでは、IIS 7 以降のバージョンで利用できるのみです。
> - 使用*オフライン展開*します。 このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、サーバー上に web パッケージのコピーし IIS マネージャーからインポートする必要があります手動でします。
> 
> 主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。


特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する場合ははい。 この方法は、これらの種類のシナリオでは望ましくは多くの場合です。

- ステージング環境または運用環境、リモート展開をトリガーする人物またはサービス アカウントは、サーバー管理者の資格情報にアクセスする可能性があります。
- リモート ユーザーが web サーバー (または他のユーザーの web サイトへのアクセス) のフル コントロールを与えることがなく、web サイトを更新できるようにするホスト環境です。

開発またはテストのシナリオまたは小規模な組織では、サーバー管理者の資格情報を使用してコンテンツの展開は、多くの場合、少ない頻度。 これらのシナリオで使用した展開をサポートするために web サーバーの構成、[デプロイのリモート エージェント サービスを Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)より直接的な方法を提供します。

## <a name="task-overview"></a>タスクの概要

そのまま使用し、Web 配置ハンドラーのアプローチを使用してリモート コンピューターから web パッケージを配置する web サーバーを構成するには、する必要があります。

- 作成、または展開の実行に使用する資格情報を持つドメイン ユーザー アカウント (「管理者以外のユーザー」) を選択します。
- IIS 7.5、Web 管理サービスなど、基本的な認証モジュールをインストールします。
- Web Deploy 2.1 以降をインストールします。
- リモートの接続を許可する Web 管理サービスを構成し、サービスを開始します。
- 展開されたコンテンツをホストする IIS の web サイトを作成します。
- IIS マネージャーで web サイトに管理者以外のユーザーのアクセス許可を付与します。
- 委任規則を追加し、管理者以外のユーザー アカウントを使用して web サイトのコンテンツの変更をサービスに許可する Web 管理サービスを確認します。
- ポート 8172 で着信接続を許可するファイアウォールを構成します。

ContactManager サンプル ソリューションを具体的にはホストにもする必要があります。

- .NET Framework 4.0 をインストールします。
- ASP.NET MVC 3 をインストールします。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 を実行しているサーバーのクリーン ビルドを開始することを想定しています。 続行する前にいることを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーは、ドメインに参加しています。
- サーバーは、静的 IP アドレスを持ちます。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。 静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。


## <a name="install-products-and-components"></a>製品とコンポーネントをインストールします。

このセクションで、web サーバーで必要な製品とコンポーネントのインストールを説明します。 開始する前に、サーバーが完全に最新であることを確認する Windows 更新プログラムを実行することをお勧めは。

この場合、これらをインストールする必要があります。

- **IIS 7 推奨構成**します。 これにより、 **Web サーバー (IIS)** web サーバー上のロールと IIS モジュールと ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。
- **IIS: 管理サービス**します。 これにより、IIS で Web 管理サービス (WMSvc) がインストールされます。 このサービスは、IIS web サイトのリモート管理を有効にし、クライアントに Web 配置ハンドラーのエンドポイントを公開します。
- **IIS: 基本認証**します。 これは、IIS 基本認証モジュールをインストールします。 これにより、Web 管理サービス (WMSvc) は、指定した資格情報を認証します。
- **Web 配置ツール 2.1 以降**します。 これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。 このプロセスの一環としては、Web 配置ハンドラーをインストールし、Web 管理サービスを統合します。
- **.NET framework 4.0**します。 これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。
- **ASP.NET MVC 3**します。 MVC 3 アプリケーションを実行する必要があるアセンブリがインストールされます。

> [!NOTE]
> このチュートリアルでは、さまざまなコンポーネントをインストールして構成、Web Platform Installer の使用について説明します。 Web Platform Installer を使用する必要はありません、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。 詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)します。


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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. **ASP.NET MVC 3 (Visual Studio 2010)** 行で、をクリックして**追加**します。
7. ナビゲーション ウィンドウで、 **Server**します。
8. **IIS 7 推奨構成**行で、をクリックして**追加**します。
9. **Web 配置ツール 2.1**行で、をクリックして**追加**します。
10. **IIS: 基本認証**行で、をクリックして**追加**します。
11. **IIS: 管理サービス**行で、をクリックして**追加**します。
12. **[インストール]** をクリックします。 Web Platform Installer が製品の一覧を表示&#x2014;と関連する依存関係&#x2014;をインストールし、ライセンス条項に同意するように求められます。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. ライセンス条項を確認し、条項に同意する場合にクリックします**同意**します。
14. インストールが完了したら、クリックして**完了**、閉じて、 **Web Platform Installer 3.0**ウィンドウ。

IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS に ASP.NET の最新バージョンを登録します。 これを行わないと、見つかります IIS が静的なコンテンツを提供 (HTML ファイル) など、問題なくが返されます**HTTP エラー 404.0 – 見つかりません**しようとすると、ASP.NET のコンテンツを参照します。 次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認します。

**ASP.NET 4.0 を IIS に登録するには**

1. クリックして**開始**、し、入力**コマンド プロンプト**します。
2. 右クリックし、検索結果で**コマンド プロンプト**、 をクリックし、**管理者として実行**します。
3. コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. 任意の時点の 64 ビット web アプリケーションをホストする場合は、IIS と ASP.NET の 64 ビット バージョンを登録することも必要があります。 コマンド プロンプト ウィンドウに移動します。、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。
6. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

お勧めとして Windows Update を使用してもう一度この時点でダウンロードして、新製品とコンポーネントをインストールしたら、使用可能な更新プログラムをインストールします。

## <a name="configure-the-web-management-service"></a>Web 管理サービスを構成します。

これで必要なものすべてをインストールしたら、次の手順は、IIS で Web 管理サービスを構成するは。 大まかに言えば、これらのタスクを完了する必要があります。

- サーバー レベルで基本認証を有効にします。
- リモート接続を受け入れる Web 管理サービスを構成します。
- Web 管理サービスを開始します。
- 必要な Web 管理サービス委任規則の確立されていることを確認します。

**Web 管理サービスを構成するには**

1. **開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。
2. IIS マネージャーで、**接続**ウィンドウで、サーバー ノードをクリックします (たとえば、 **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. 中央のウィンドウで  **IIS**、ダブルクリックして**認証**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. 右クリック**基本認証**、 をクリックし、**を有効にする**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. **接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。
6. 中央のウィンドウで **管理**、ダブルクリックして**管理サービス**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. 中央のウィンドウで次のように選択します。**リモート接続を有効にする**します。

    > [!NOTE]
    > Web 管理サービスが既に実行されている場合は、まずこれを停止する必要があります。
8. **アクション**ウィンドウで、をクリックして**開始**Web 管理サービスを開始します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. 設定を保存するメッセージが表示されたら、クリックして**はい**します。

    > [!NOTE]
    > 自動的に開始するサービスを構成することもできます。 これを行うには、サービス コンソールを開きを右クリックして**Web 管理サービス**、 をクリックし、**プロパティ**します。 **スタートアップの種類**ドロップダウン リストで、**自動**、 をクリックし、 **OK**。
10. **接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。
11. 中央のウィンドウで **管理**、ダブルクリックして**管理サービスの委任**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. 中央のウィンドウにはで、一連ルールにはが含まれていることを確認します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    これらの規則は、さまざまな Web 配置プロバイダーを使用する承認された Web 管理サービスのユーザーを許可します。 たとえば、web アプリケーションとコンテンツを Web 配置ハンドラーを使用して IIS を展開する必要がありますすべてを使用する Web 管理サービスのユーザーを認証できるようにする委任規則、 **contentPath**と**iisApp**プロバイダー (スクリーン ショットに表示される最後のルール)。

    このトピックで説明されている順序でプロジェクトとコンポーネントをインストールした場合最新バージョンの Web 配置 Web 管理サービスをすべての必要な委任規則を追加する必要がありますは自動的にします。 管理サービスの委任 ページで、規則が表示されない場合は、自分で作成する必要があります。 これを行う方法の詳細については、次を参照してください。 [Web 配置ハンドラーを構成する](https://go.microsoft.com/?linkid=9805124)します。
13. **接続**ウィンドウで、最上位レベルの設定に戻るにもう一度サーバー ノードをクリックします。

## <a name="create-and-configure-an-iis-website"></a>作成し、IIS の web サイトを構成します。

Web コンテンツを展開するには、サーバーに、前に作成してコンテンツをホストする IIS の web サイトを構成する必要があります。 Web Deploy; 既存の IIS web サイトに web パッケージを配置できるだけweb サイトを作成できません。 また、コンテンツをリモートで展開を非管理者のアカウントを許可するほとんどの余分な構成を行う必要があります。 大まかに言えば、これらのタスクを完了する必要があります。

- コンテンツをホストするファイル システム上のフォルダーを作成します。
- コンテンツを処理するために IIS の web サイトを作成し、ローカル フォルダーに関連付けます。
- 読み取りローカル フォルダーにアプリケーション プール id へのアクセス許可を付与します。
- Web アプリケーションを展開するドメイン アカウントに必要な IIS アクセス許可を付与します。

IIS の既定の web サイトにコンテンツを展開するなんらですが、この方法はテストまたはデモ シナリオ以外は推奨されません。 運用環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。

**IIS の web サイトを作成するには**

1. ローカル ファイル システムでコンテンツを格納するフォルダーを作成します (たとえば、 **C:\DemoSite**)。
2. **開始**メニューで、**管理ツール**、 をクリックし、**インターネット インフォメーション サービス (IIS) マネージャー**します。
3. IIS マネージャーで、**接続**ウィンドウで、サーバー ノードを展開 (たとえば、 **STAGEWEB1**)。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. 右クリックし、**サイト**ノード、およびクリック**Web サイトの追加**します。
5. **サイト名**ボックスに、IIS web サイトの名前を入力 (たとえば、 **DemoSite**)。
6. **物理パス**ボックス、入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。
7. **ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。

    > [!NOTE]
    > 標準ポート番号は、http 80 および HTTPS の場合は 443 です。 ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。
8. ままに、**ホスト名**ボックスをクリックして、web サイト用のドメイン ネーム システム (DNS) レコードを構成する場合、空白**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > 運用環境では、ポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成するします可能性があります。 IIS 7 でホスト ヘッダーの構成の詳細については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/library/cc753195(WS.10).aspx)します。 Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/windowsserver/dd448607)します。
9. **[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。
10. **サイト バインド**ダイアログ ボックスで、をクリックして**追加**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. **サイト バインドの追加**ダイアログ ボックスで、セット、 **IP アドレス**と**ポート**既存のサイトの構成に一致するようにします。
12. **ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **STAGEWEB1**)、順にクリックします**OK**。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > 最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`します。 2 つ目のサイトのバインドでは、マシン名を使用して、ドメインの他のコンピューターからサイトにアクセスできます (たとえば、 http://stageweb1:85) します。
13. **サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**します。
14. **接続**ウィンドウで、をクリックして**アプリケーション プール**します。
15. **アプリケーション プール**ウィンドウは、アプリケーション プールの名前を右クリックし、クリックして**基本設定**します。 既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。
16. **.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリックします**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > サンプル ソリューションでは、.NET Framework 4.0 が必要です。 これではなく Web デプロイのための要件です。

Web サイト コンテンツを配信するためには、アプリケーション プール id をコンテンツを格納するローカル フォルダーのアクセス許可読み取りが必要です。 IIS 7.5 では、アプリケーション プールは、(アプリケーション プールが通常実行されるネットワーク サービス アカウントを使用して、IIS の以前のバージョン) とは異なり、既定で一意のアプリケーション プール id で実行します。 アプリケーション プール id の実際のユーザー アカウントではないとユーザーまたはグループのすべてのリストが表示されない&#x2014;代わりにときに、作成されます動的にアプリケーション プールを開始します。 各アプリケーション プール id をローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示の項目として。

ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。

- アクセス許可を割り当てる、アプリケーション プール id に直接、形式を使用して<strong>IIS AppPool\</strong ><em>[アプリケーション プール名]</em>(たとえば、 <strong>IIS AppPool\DemoSite</strong>).
- アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。

最も一般的なアプローチは、ローカルに権限を割り当てる、 **IIS\_IUSRS**グループ、このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更できるためです。 次の手順では、このグループ ベースのアプローチを使用します。

> [!NOTE]
> IIS 7.5 でアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。


**IIS の web サイトのフォルダーのアクセス許可を構成するには**

1. Windows エクスプ ローラーでは、ローカル フォルダーの場所を参照します。
2. フォルダーを右クリックし、**プロパティ**します。
3. **セキュリティ**] タブで [**編集**、順にクリックします**追加**します。
4. クリックして**場所**します。 **場所** ダイアログ ボックスでは、ローカルのサーバーを選択し、順にクリックします**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. **[ユーザーまたはグループ**ダイアログ ボックスに「 **IIS\_IUSRS**、] をクリックして**名前の確認**順にクリックします**OK**します。
6. <strong>のアクセス許可</strong><em>[フォルダー名]</em>ダイアログ ボックスで、新しいグループが割り当てられている通知、<strong>読み取り&amp;実行</strong>、<strong>フォルダーの一覧内容</strong>、および<strong>読み取り</strong>既定のアクセスを許可します。 変更なしのままにし、クリックして<strong>OK</strong>します。
7. をクリックして<strong>OK</strong>を閉じる、 <em>[フォルダー名]</em><strong>プロパティ</strong> ダイアログ ボックス。

最後のタスクとしては、管理者以外のコンテンツ展開に使用する資格情報を持つユーザーに適切なアクセス許可を与える必要があります。 このユーザーには、web サイトにコンテンツをリモートで展開するアクセス許可が必要です。

**管理者以外のドメイン ユーザーの IIS web サイトのアクセス許可を構成するには**

1. IIS マネージャーで、**接続**ウィンドウで、web サイト ノードを右クリックして (たとえば、 **DemoSite**)、 をポイント**デプロイ**、順にクリックします**Web の構成配置発行**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. **Web 配置発行の構成**の右側に、ダイアログ ボックス、**公開アクセス許可を付与するユーザーを選択**一覧で、省略記号ボタンをクリックします。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. **を許可するユーザー**  ダイアログ ボックスをクリックして、コンテンツの展開を使用するアカウントのドメインとユーザーの名前を入力**OK**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. **Web 配置発行の構成**ダイアログ ボックスで、をクリックして**セットアップ**します。

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > この操作は、1 つの手順で 2 つの主要機能を実行します。 最初に、前のセクションでを調べ、委任規則に従って、Web 管理サービスを使用してリモートでの web サイトを変更するユーザーのアクセス許可を付与します。 次に、追加、変更、および web サイトのコンテンツにアクセス許可を設定するユーザーは、web サイトのソース フォルダーのユーザーのフル コントロールを付与します。
5. **Web 配置発行の構成**ダイアログ ボックスで、をクリックして**閉じる**します。

## <a name="configure-firewall-exceptions"></a>ファイアウォールの例外を構成します。

既定では、IIS の Web 管理サービスは TCP ポート 8172 でリッスンします。 Web サーバーで Windows ファイアウォールが有効な場合は、新しい受信規則をポート 8172 で TCP トラフィックを許可する (すべての送信トラフィックは既定では Windows ファイアウォールの許可) を作成する必要があります。 サードパーティ製のファイアウォールを使用する場合は、トラフィックを許可するルールを作成する必要があります。

| Direction | ポートから | ポートに | ポートの種類 |
| --- | --- | --- | --- |
| 受信 | どれでも可 | 8172 | TCP |
| 送信 | 8172 | どれでも可 | TCP |
  

Windows ファイアウォールのルールの構成の詳細については、次を参照してください。[ファイアウォール規則を構成する](https://technet.microsoft.com/library/dd448559(WS.10).aspx)します。 サードパーティ製のファイアウォール、製品のマニュアルを参照してください。

## <a name="conclusion"></a>まとめ

Web サーバーは、Web 管理サービスを使用して、Web 配置ハンドラーへのリモート展開をそのまま使用できるようになります。 サーバーに web アプリケーションをデプロイする前に、次の点を確認したい場合があります。

- IIS でサーバー レベルで基本認証を有効にしますか。
- Web 管理サービスへのリモート接続を有効にしますか。
- Web 管理サービスを開始するか。
- 管理サービス委任規則場所か。
- アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスをあるでしょうか。
- 管理者以外のユーザー アカウントはサイト レベルのアクセス許可を IIS でありますか。
- ファイアウォールは TCP ポート 8172 でサーバーへの着信接続を許可しますか。

## <a name="further-reading"></a>関連項目

Web 配置ハンドラーに web パッケージを配置するカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)します。

> [!div class="step-by-step"]
> [前へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
