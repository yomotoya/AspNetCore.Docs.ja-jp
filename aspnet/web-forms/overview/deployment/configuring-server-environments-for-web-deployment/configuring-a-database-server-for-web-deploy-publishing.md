---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Web のデータベース サーバーを構成する配置の発行 |Microsoft Docs
author: jrjlee
description: このトピックでは、web 配置および発行をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。 このトピックで説明するタスクは、co.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 0f8639efcfcd02c9fdb65ce3ed25272965be8aa8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826883"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Web 配置発行のデータベース サーバーを構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web 配置および発行をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。
> 
> このトピックで説明するタスクは、すべての展開シナリオに共通&#x2014;(Web 配置) IIS Web 配置ツールのリモート エージェント サービス、Web 配置ハンドラー、またはオフライン展開を使用して、web サーバーが構成されているかにかかわらず、またはアプリケーションが 1 つの web サーバーまたはサーバー ファームで実行します。 データベースをデプロイする方法は、セキュリティ要件とその他の考慮事項に従って変更できます。 たとえば、データベース、サンプル データの有無を展開する場合があり、ユーザー ロールのマッピングを展開または展開後に手動で構成する場合があります。 ただし、データベース サーバーを構成する方法は同じです。


Web 配置をサポートするためにデータベース サーバーを構成する追加の製品やツールをインストールする必要はありません。 データベース サーバーと web サーバーが別々 のコンピューターで実行すると仮定すると単純にする必要があります。

- SQL Server で TCP/IP を使用して通信を許可します。
- ファイアウォール経由の SQL Server トラフィックを許可します。
- Web サーバーのコンピューター アカウント、SQL Server ログインを提供します。
- 任意の必要なデータベース ロールにマシン アカウントのログインをマップします。
- SQL Server ログインとデータベース作成者のアクセス許可、展開を実行するアカウントに与えます。
- 繰り返し展開をサポートするには展開アカウントのログインをマップ、 **db\_所有者**データベース ロール。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとチュートリアルでは、このトピックでは、Windows Server 2008 R2 で実行されている SQL Server 2008 R2 の既定のインスタンスを開始することを想定しています。 続行する前にいることを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーは、ドメインに参加しています。
- サーバーは、静的 IP アドレスを持ちます。
- SQL Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。

のみ、SQL Server インスタンスを含める必要があります、**データベース エンジン サービス**ロールは、任意の SQL Server のインストールに自動的に含まれています。 ただし、構成と保守の容易にするため、推奨を含めること、**管理ツール-基本**と**管理ツール-完全**サーバーの役割。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューターのドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)します。 静的 IP アドレスの構成の詳細については、次を参照してください。[静的 IP アドレス構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)します。 SQL Server をインストールする方法の詳細については、次を参照してください。 [SQL Server 2008 R2 のインストール](https://technet.microsoft.com/library/bb500395.aspx)します。


## <a name="enable-remote-access-to-sql-server"></a>SQL Server へのリモート アクセスを有効にします。

SQL Server では、TCP/IP を使用してリモート コンピューターと通信します。 データベース サーバーと web サーバーが異なるコンピューター上にある場合は、する必要があります。

- TCP/IP 経由で通信を許可する SQL Server ネットワークの設定を構成します。
- TCP トラフィックを許可する (および場合によってはユーザー データグラム プロトコル (UDP) トラフィック) に SQL Server インスタンスが使用するポートでハードウェアまたはソフトウェア ファイアウォールを構成します。

TCP/IP 経由で通信するのに SQL Server を有効にするのにには、SQL Server インスタンスのネットワーク構成を変更するのに SQL Server 構成マネージャーを使用します。

**SQL Server で TCP/IP を使用して通信を有効にするには**

1. **開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、 をクリックして**構成ツール**、順にクリックします**SQL Server 構成マネージャー**します。
2. ツリー ビュー ウィンドウで [ **SQL Server ネットワーク構成**、] をクリックし、 **MSSQLSERVER のプロトコル**。

   > [!NOTE]
   > SQL Server の複数のインスタンスをインストールする場合が表示されます、<strong>プロトコル</strong><em>[インスタンス名]</em>インスタンスごとの項目。 インスタンスのインスタンスでベースでネットワーク設定を構成する必要があります。
3. 詳細ペインで右クリックし、 **TCP/IP**行をクリックして**を有効にする**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. **警告**ダイアログ ボックスで、をクリックして**OK**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. 新しいネットワーク構成を有効にするには、MSSQLSERVER サービスを再起動する必要があります。 サービス コンソールで、または SQL Server Management Studio から、コマンド プロンプトを実行できます。 この手順では、SQL Server Management Studio を使用します。
6. SQL Server 構成マネージャーを閉じます。
7. **開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、 をクリックし、 **SQL Server Management Studio**。
8. **サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、クリックして**Connect**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. **オブジェクト エクスプ ローラー**ウィンドウで、親サーバーのノードを右クリックし (たとえば、 **TESTDB1**)、順にクリックします**再起動**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. **Microsoft SQL Server Management Studio**ダイアログ ボックスで、をクリックして**はい**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. サービスが再起動されると、SQL Server Management Studio を閉じます。

ファイアウォール経由で SQL Server トラフィックを許可するのには、まず、SQL Server インスタンスを使用してポートを把握する必要があります。 これについては、SQL Server インスタンスを作成および構成する方法に依存します。

- A*既定のインスタンス*(およびに応答する) をリッスンする SQL Server の TCP ポート 1433 で要求します。
- A*名前付きインスタンス*(およびに応答する) をリッスンする SQL Server の動的に割り当てられた TCP ポートで要求します。
- SQL Server Browser サービスが有効になっている場合、クライアントは UDP ポートを特定の SQL Server インスタンスを使用する TCP ポート 1434 でサービスを照会できます。 ただし、このサービスをセキュリティ上の理由から多くの場合、無効にします。

SQL Server の既定のインスタンスを使用するいると仮定するトラフィックを許可するようにファイアウォールを構成する必要があります。

| Direction | ポートから | ポートに | ポートの種類 |
| --- | --- | --- | --- |
| 受信 | どれでも可 | 1433 | TCP |
| 送信 | 1433 | どれでも可 | TCP |
  

> [!NOTE]
> 技術的には、クライアント コンピューターは、SQL Server との通信に 1024 ~ 5000 のランダムに割り当てられた TCP ポートを使用し、それに応じてファイアウォールの規則を制限することができます。 SQL Server のポートとファイアウォールの詳細については、次を参照してください[SQL ファイアウォール経由で通信するために必要な TCP/IP ポート番号](https://go.microsoft.com/?linkid=9805125)と[方法: 特定の TCP ポート (SQL Server の構成でリッスンするようにサーバーの構成。マネージャー)](https://msdn.microsoft.com/library/ms177440.aspx)します。


ほとんどの Windows Server 環境では、データベース サーバーで Windows ファイアウォールを構成する必要あります可能性があります。 既定では、Windows ファイアウォールは、ルールによって特に禁止されている場合を除き、すべての送信トラフィックを許可します。 Web サーバーをデータベースにアクセスを有効にするには、SQL Server インスタンスが使用するポート番号で TCP トラフィックを許可する受信規則を構成する必要があります。 SQL Server の既定のインスタンスを使用している場合は、このルールを構成する次の手順を使用できます。

**既定の SQL Server インスタンスとの通信を許可するための Windows ファイアウォールを構成するには**

1. データベース サーバー上で、**開始**メニューで、**管理ツール**、順にクリックします**セキュリティが強化された Windows ファイアウォール**します。
2. ツリー ビュー ペインで次のようにクリックします。**受信の規則**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. **アクション**ウィンドウで、**受信の規則**、 をクリックして**新しいルール**します。
4. 新しい受信の規則ウィザードで、**規則の種類**] ページで、[**ポート**、順にクリックします **[次へ]** します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. **プロトコルおよびポート**いることを確認 ページで、 **TCP**が選択されていると、**特定のローカル ポート**ボックスに「 **1433**、順にクリックします**次**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. **アクション**ままにします ページで、**接続を許可する**を選択し、をクリックして**次**。
7. **プロファイル**ままにします ページで、**ドメイン**選択すると、クリア、**プライベート**と**パブリック**チェック ボックスをクリックして**次へ**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. **名前** ページで、適切にわかりやすい名前を規則に付けます (たとえば、 **SQL Server の既定のインスタンス: ネットワーク アクセス**)、順にクリックします**完了**。

参照してください、標準または動的ポート経由で SQL Server と通信する必要がある場合は特に、SQL Server 用 Windows ファイアウォールの構成の詳細については[方法: データベース エンジン アクセスの Windows ファイアウォールを構成する](https://technet.microsoft.com/library/ms175043.aspx)します。

## <a name="configure-logins-and-database-permissions"></a>ログインを構成し、データベース権限

Web アプリケーションにインターネット インフォメーション サービス (IIS) を展開するときに、アプリケーション プールの id を使用して、アプリケーションが実行されます。 ドメイン環境では、アプリケーション プール id は、ネットワーク リソースにアクセスを実行するサーバーのコンピューター アカウントを使用します。 コンピューター アカウントとして具現化<em>[ドメイン名]</em><strong>\</strong ><em>[machine name]</em><strong>$</strong>&#x2014;など<strong>FABRIKAM\TESTWEB1$</strong>します。 Web アプリケーションをネットワーク経由でデータベースへのアクセスを許可するのには、する必要があります。

- SQL Server インスタンスには、web サーバーのコンピューター アカウントのログインを追加します。
- 任意の必要なデータベース ロールにマシン アカウントのログインにマップ (通常**db\_datareader**と**db\_datawriter の各**)。

Web アプリケーションが 1 台のサーバーではなく、サーバー ファームで実行している場合は、サーバー ファーム内のすべての web サーバーにこれらの手順を繰り返す必要があります。

> [!NOTE]
> アプリケーション プール id とネットワークのリソースへのアクセスの詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)します。


さまざまな方法でこれらのタスクの方法があります。 ログインを作成するには、ことができますか。

- TRANSACT-SQL または SQL Server Management Studio を使用して、データベース サーバーに手動でログインを作成します。
- Visual Studio で SQL Server 2008 のサーバー プロジェクトを使用して、作成して、ログインをデプロイします。

SQL Server ログインは、ために展開するデータベースに依存することはできません、データベース レベルのオブジェクトではなく、サーバー レベル オブジェクトです。 そのため、任意の時点では、ログインを作成して、データベースの展開を開始する前に、データベース サーバーのログインを手動で作成する最も簡単な方法が多くの場合は。 次の手順を使用して、SQL Server Management Studio でのログインを作成することができます。

**Web サーバー コンピューター アカウントに SQL Server ログインを作成するには**

1. データベース サーバー上で、**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、順にクリックします**SQL Server Management Studio**.
2. **サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、クリックして**Connect**します。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. **オブジェクト エクスプ ローラー**ウィンドウで、右クリックして**セキュリティ**、 をポイント**新規**、順にクリックします**ログイン**します。
4. **ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、web サーバーのコンピューター アカウントの名前を入力 (たとえば、 **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. **[OK]** をクリックします。

この時点で、データベース サーバーが Web 配置発行できます。 ただし、必要なデータベース ロールにマシン アカウントのログインをマップするまで、デプロイする任意のソリューションは機能しません。 考えるより多くのデータベース ロールにログインにマッピングが必要ですとすることはできません後まで、マップのロール デプロイしたデータベース。 必要なデータベース ロールにマシン アカウントのログインをマップするには、ことができますか。

- データベース ロール、ログインに手動で割り当てる、最初にデータベースをデプロイした後。
- 配置後スクリプトを使用して、ログインにデータベース ロールを割り当てます。

ログインとデータベース ロールのマッピングの作成を自動化する方法については、次を参照してください。[テスト環境にデータベース ロール メンバーシップを展開する](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)します。 または、次の手順を使用すると、マシン アカウントのログインを手動で必要なデータベース ロールにマップします。 までには、この手順を実行することはできませんに注意してください。*後*データベースをデプロイしました。

**データベース ロールを web サーバー マシン アカウントのログインにマップするには**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックします (たとえば、 **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. **ログインのプロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**します。
4. **このログインにマップされたユーザー**テーブルで、データベースの名前を選択します (たとえば、 **ContactManager**)。
5. **データベース ロールのメンバーシップ:** *[データベース名]* 一覧で、必要なアクセス許可を選択します。 選択する必要があります Contact Manager のサンプル ソリューションの場合、 **db\_datareader**と**db\_datawriter の各**ロール。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. **[OK]** をクリックします。

データベース ロールの手動マッピングは、テスト環境の複数の適切なが多くの場合、はステージング環境または運用環境への自動、または 1 回のクリックの展開の推奨されません。 詳細についてはこの種の配置後スクリプトを使用してタスクを自動化することで見つかります[テスト環境にデータベース ロール メンバーシップを展開する](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)します。

> [!NOTE]
> サーバー プロジェクトとデータベース プロジェクトの詳細については、次を参照してください。 [Visual Studio 2010 の SQL Server データベース プロジェクト](https://msdn.microsoft.com/library/ff678491.aspx)します。


## <a name="configure-permissions-for-the-deployment-account"></a>展開アカウントのアクセス許可を構成します。

展開を実行するに使用するアカウントが SQL Server の管理者でない場合は、このアカウントのログインを作成する必要がありますをも。 メンバーである必要があります、データベースを作成するには、アカウント、 **dbcreator**サーバー ロールまたは同等のアクセス許可があります。

> [!NOTE]
> データベースを配置する Web デプロイまたは VSDBCMD を使用する場合は、(SQL Server インスタンスが混合モード認証をサポートするために構成されている) 場合、Windows 資格情報または SQL Server 資格情報を使用できます。 次の手順では、Windows 資格情報を使用したいもなんら SQL Server ユーザー名とパスワードの展開を構成するときに、接続文字列の指定を使用する必要があること前提としています。


**展開アカウントのアクセス許可を設定するには**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**ウィンドウで、右クリックして**セキュリティ**、 をポイント**新規**、順にクリックします**ログイン**します。
3. **ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、展開アカウントの名前を入力 (たとえば、 **FABRIKAM\matt**)。
4. **ページの選択**ウィンドウで、をクリックして**サーバーの役割**します。
5. 選択**dbcreator**、 をクリックし、 **OK**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

以降のデプロイをサポートするためにもする必要があります展開アカウントを追加、 **db\_所有者**の最初のデプロイ後に、データベース ロール。 以降のデプロイで、既存のデータベースのスキーマの変更はなくする新しいデータベースを作成するためです。 前のセクションで説明した、明確な理由により、データベースを作成するまで、データベース ロールにユーザーを追加することはできません。

**データベースに展開アカウントのログインにマップする\_データベース ロールの所有者**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックします (たとえば、 **FABRIKAM\matt**)。
3. **ログインのプロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**します。
4. **このログインにマップされたユーザー**テーブルで、データベースの名前を選択します (たとえば、 **ContactManager**)。
5. **データベース ロールのメンバーシップ:** *[データベース名]* 一覧で、、 **db\_所有者**ロール。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. **[OK]** をクリックします。

## <a name="conclusion"></a>まとめ

データベース サーバーは、リモート データベースの配置をそのまま使用し、リモートの IIS web サーバー、データベースにアクセスを許可するようになります。 展開データベースを使用する前に、次の点を確認したい場合があります。

- リモートの TCP/IP 接続を許可する SQL Server の構成
- SQL Server トラフィックを許可するファイアウォールを構成しましたか。
- SQL Server にアクセスするすべての web サーバーのマシン アカウント ログインを作成しましたか。
- データベースの配置は、ユーザー ロールのマッピングを作成するスクリプトを含めます後、最初にデータベースを配置する手動で作成する必要がありますか。
- 展開アカウントのログインを作成して追加して、 **dbcreator**サーバーの役割ですか?

## <a name="further-reading"></a>関連項目

データベース プロジェクトの配置方法の詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。 配置後スクリプトを実行してデータベース ロールのメンバーシップを作成する方法の詳細については、次を参照してください。[テスト環境にデータベース ロール メンバーシップを展開する](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)します。 メンバーシップ データベースをもたらす独自の展開の課題に対応する方法のガイダンスについては、次を参照してください。[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)します。

> [!div class="step-by-step"]
> [前へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [次へ](creating-a-server-farm-with-the-web-farm-framework.md)
