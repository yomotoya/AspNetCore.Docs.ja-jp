---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: デプロイの発行設定 Web のデータベース サーバーの構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、web デプロイおよび公開をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。 このトピックで説明するタスクは、co.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: a2340c0d561ed274e281b5f6d942af0a2027315a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Web デプロイの発行用のデータベース サーバーの構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web デプロイおよび公開をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。
> 
> このトピックで説明するタスクは、すべての展開シナリオに共通&#x2014;IIS Web 配置ツール (Web 配置) のリモート エージェント サービス、Web 配置ハンドラー、またはオフラインの展開を使用する web サーバーが構成されているかにかかわらず、またはアプリケーションが 1 つの web サーバーまたはサーバー ファームで実行します。 セキュリティ要件およびその他の注意事項に応じて、データベースを展開する方法を変更ことがあります。 たとえば、サンプル データの有無、データベースを展開する場合があり、ユーザー ロールのマッピングを展開または展開後に手動で構成する場合があります。 ただし、データベース サーバーを構成する方法は変わりません。


Web 配置をサポートするためにデータベース サーバーを構成する追加の製品やツールをインストールする必要はありません。 想定されるので、データベース サーバーと web サーバーは、別のコンピューターで実行、単純にする必要があります。

- SQL Server で TCP/IP を使用して通信を許可します。
- SQL Server のファイアウォールを通過するトラフィックを許可します。
- Web サーバーのコンピューター アカウントに SQL Server ログインを付けます。
- 任意の必要なデータベース ロールにマシン アカウントのログインをマップします。
- SQL Server ログインとデータベース作成者のアクセス許可を展開を実行するアカウントに与えます。
- 繰り返し展開をサポートするために、展開アカウントへのログインをマップ、 **db\_所有者**データベース ロール。

このトピックでは、各手順を実行する方法を示します。 タスクとここでチュートリアルは、Windows Server 2008 R2 で実行されている SQL Server 2008 R2 の既定のインスタンスを開始していることを想定しています。 続行する前に以下のことを確認します。

- Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。
- サーバーがドメインに参加します。
- サーバーは、静的 IP アドレスを持ちます。
- SQL Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。

のみ、SQL Server インスタンスを含める必要がある、**データベース エンジン サービス**ロールは、任意の SQL Server のインストールに自動的に含まれています。 ただし、構成と保守の容易にするため、推奨を含めること、**管理ツール-基本**と**管理ツール-完全**サーバーの役割です。

> [!NOTE]
> コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx)です。 静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx)です。 SQL Server のインストールの詳細については、次を参照してください。 [SQL Server 2008 R2 のインストール](https://technet.microsoft.com/library/bb500395.aspx)です。


## <a name="enable-remote-access-to-sql-server"></a>SQL Server へのリモート アクセスを有効にします。

SQL Server では、リモート コンピューターと通信するために TCP/IP が使用されます。 データベース サーバーと web サーバーが異なるコンピューター上にある場合は、する必要があります。

- TCP/IP 経由で通信を許可する SQL Server ネットワークの設定を構成します。
- TCP トラフィックを許可する (および場合によってはユーザー データグラム プロトコル (UDP) トラフィック) に、SQL Server インスタンスを使用するポートで、ハードウェアまたはソフトウェア ファイアウォールを構成します。

TCP/IP 経由で通信するために SQL Server を有効にするのにには、SQL Server インスタンスのネットワーク構成を変更するのに SQL Server 構成マネージャーを使用します。

**SQL Server で TCP/IP を使用して通信を有効にするには**

1. **開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、 をクリックして**構成ツール**をクリックし、**SQL Server 構成マネージャー**です。
2. ツリー ビュー ペインで、展開**SQL Server ネットワーク構成**、クリックして**MSSQLSERVER のプロトコル**です。

   > [!NOTE]
   > SQL Server の複数のインスタンスをインストールする場合が表示されます、<strong>プロトコル</strong><em>[インスタンス名]</em>インスタンスごとの項目。 インスタンスで、インスタンスごとにネットワーク設定を構成する必要があります。
3. 詳細ウィンドウで右クリックし、 **TCP/IP**行をクリックして**を有効にする**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. **警告**ダイアログ ボックスで、をクリックして**OK**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. 新しいネットワーク構成を有効にするには、MSSQLSERVER サービスを再起動する必要があります。 サービス コンソールで、または SQL Server Management Studio から、コマンド プロンプトでを実行できます。 この手順では SQL Server Management Studio を使用します。
6. SQL Server 構成マネージャーを閉じます。
7. **開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、クリックして**SQL Server Management Studio**です。
8. **サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、をクリックして**接続**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. **オブジェクト エクスプ ローラー**  ウィンドウで、親サーバー ノードを右クリックして (たとえば、 **TESTDB1**)、をクリックし、**再起動**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. **Microsoft SQL Server Management Studio**ダイアログ ボックスで、をクリックして**はい**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. サービスが再起動されたときに、SQL Server Management Studio を閉じます。

SQL Server ファイアウォールを通過するトラフィックを許可するのには、まず、SQL Server インスタンスを使用するポートを知っている必要があります。 これは、SQL Server インスタンスを作成および構成方法に依存は。

- A*既定のインスタンス*SQL Server の受信 (およびに応答) の TCP ポート 1433 で要求します。
- A*名前付きインスタンス*SQL Server のリッスン (し応答する)、動的に割り当てられる TCP ポートで要求します。
- SQL Server Browser サービスが有効になっている場合、クライアントは UDP ポート 1434 を特定の SQL Server インスタンスを使用する TCP ポートを調べるには上のサービスをクエリできます。 ただし、このサービスは、多くの場合、セキュリティの理由により無効です。

SQL Server の既定のインスタンスを使用するいると想定されるので、トラフィックを許可するようにファイアウォールを構成する必要があります。

| Direction | ポートから | ポートに | ポートの種類 |
| --- | --- | --- | --- |
| 受信 | どれでも可 | 1433 | TCP |
| 送信 | 1433 | どれでも可 | TCP |
  

> [!NOTE]
> 技術的には、クライアント コンピューターは、SQL Server との通信に 1024 ~ 5000 でランダムに割り当てられた TCP ポートを使用し、それに従ってファイアウォールのルールを制限することができます。 SQL Server のポートとファイアウォールの詳細については、次を参照してください[SQL ファイアウォール経由で通信するために必要な TCP/IP ポート番号](https://go.microsoft.com/?linkid=9805125)と[する方法: 特定の TCP ポート (SQL Server の構成でリッスンするようにサーバーを構成する。Manager)](https://msdn.microsoft.com/library/ms177440.aspx)です。


ほとんどの Windows Server 環境では、可能性があります、データベース サーバーで Windows ファイアウォールを構成する必要があります。 既定では、Windows ファイアウォールは、ルールによって特に禁止されている場合を除き、すべての送信トラフィックを許可します。 Web サーバーをデータベースにアクセスを有効にするには、SQL Server インスタンスが使用するポート番号で TCP トラフィックを許可する受信規則を構成する必要があります。 SQL Server の既定のインスタンスを使用している場合は、このルールを構成する次の手順を使用できます。

**既定の SQL Server インスタンスとの通信を許可するための Windows ファイアウォールを構成するには**

1. データベース サーバー上で、**開始** メニューのをポイント**管理ツール**、順にクリック**セキュリティが強化された Windows ファイアウォール**です。
2. ツリー ビュー ウィンドウでをクリックして**受信の規則**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. **アクション**ペイン、下にある**受信の規則**、 をクリックして**新しいルール**です。
4. 新規の受信の規則ウィザードでの**規則の種類**] ページで、[**ポート**、順にクリック**[次へ]**。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. **プロトコルおよびポート**いることを確認] ページで、 **TCP**が選択されているし、[、**特定のローカル ポート**ボックスに、入力**1433**をクリックし、**次**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. **アクション** ページのままに**接続を許可する**を選択し、をクリックして**次**です。
7. **プロファイル** ページのままにして**ドメイン**選択すると、オフ、**プライベート**と**パブリック**チェック ボックスをオンし、をクリックして**次へ**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. **名** ページで、ルールに適切にわかりやすい名前を付けます (たとえば、 **SQL Server の既定のインスタンス: ネットワーク アクセス**)、をクリックし、 **完了**です。

SQL Server と標準または動的ポート経由の通信を参照して必要がある場合に特に、SQL Server 用 Windows ファイアウォールを構成する方法についての[する方法: データベース エンジン アクセスの Windows ファイアウォールを構成する](https://technet.microsoft.com/library/ms175043.aspx)です。

## <a name="configure-logins-and-database-permissions"></a>ログインを構成し、データベース権限

Web アプリケーションにインターネット インフォメーション サービス (IIS) を展開するときに、アプリケーション プールの id を使用して、アプリケーションが実行されます。 ドメイン環境では、アプリケーション プール id は、ネットワーク リソースにアクセスするが、実行するサーバーのマシン アカウントを使用します。 コンピューター アカウントは、形式をとる<em>[ドメイン名]</em><strong>\</strong ><em>[マシン名]</em><strong>$</strong>&#x2014;など<strong>FABRIKAM\TESTWEB1$</strong>します。 Web アプリケーションをネットワーク経由で、データベースへのアクセスを許可するのには、する必要があります。

- SQL Server インスタンスに、web サーバーのコンピューター アカウントのログインを追加します。
- 必要なデータベース ロールにマシン アカウントのログインをマップ (通常**db\_datareader**と**db\_datawriter**)。

Web アプリケーションが 1 台のサーバーではなく、サーバー ファームで実行されている場合は、サーバー ファーム内のすべての web サーバーに対してこれらの手順を繰り返す必要があります。

> [!NOTE]
> アプリケーション プールの id とネットワークのリソースへのアクセスの詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。


さまざまな方法でこれらのタスクを得ることができます。 ログインを作成するには、方法があります。

- TRANSACT-SQL または SQL Server Management Studio を使用して、データベース サーバーで手動でログインを作成します。
- Visual Studio で作成し、ログインを展開する SQL Server 2008 サーバー プロジェクトを使用します。

SQL Server ログインを展開するデータベースに依存しないように、データベース レベルのオブジェクトではなく、サーバー レベル オブジェクトです。 そのため、任意の時点では、ログインを作成することができ、データベースのデプロイを開始する前に、データベース サーバーでログインを手動で作成する最も簡単な方法が多くの場合は。 次の手順を使用して、SQL Server Management Studio でのログインを作成することができます。

**Web サーバー コンピューター アカウントに SQL Server ログインを作成するには**

1. データベース サーバー上で、**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**をクリックし、 **SQL Server Management Studio**.
2. **サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、をクリックして**接続**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. **オブジェクト エクスプ ローラー**  ウィンドウを右クリックして**セキュリティ**、 をポイント**新規**、クリックして**ログイン**です。
4. **ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、web サーバー マシン アカウントの名前を入力 (たとえば、 **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. **[OK]**をクリックします。

この時点では、データベース サーバーは、Web Deploy の発行可能な状態です。 ただし、展開するソリューションは、必要なデータベース ロールにマシン アカウントのログインをマップするまでに動作しません。 考えるより多くのデータベース ロールにログインにマッピングが必要です、することはできません後まで、マップのロール配置したデータベースです。 必要なデータベース ロールにマシン アカウントのログインをマップするには、方法があります。

- データベース ロールを割り当てるログインに手動で、最初にデータベースを配置した後にします。
- 配置後スクリプトを使用して、ログインにデータベース ロールを割り当てます。

ログインとデータベース ロールのマッピングの作成を自動化する方法については、次を参照してください。[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。 代わりに、次の手順を使用して、必要なデータベース ロールに手動でコンピューター アカウントのログインをマップすることができます。 までこの手順を実行することはできませんに注意してください*後*データベースを配置しました。

**データベース ロールを web サーバーのマシン アカウントのログインにマップするには**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**  ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックし、(たとえば、 **FABRIKAM\TESTWEB1$**)。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. **ログイン プロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**です。
4. **このログインにマップされたユーザー**テーブルで、データベースの名前を選択 (たとえば、 **ContactManager**)。
5. **データベース ロールのメンバーシップ:** *[データベース名]*一覧で、必要なアクセス許可を選択します。 選択する必要があります、連絡先のマネージャーのサンプル ソリューションの場合、 **db\_datareader**と**db\_datawriter**ロール。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. **[OK]**をクリックします。

データベース ロールを手動でマッピングでは、テスト環境の複数の適切な多くの場合は、ステージング環境または実稼働環境に自動または 1 回のクリックは展開の望ましいです。 詳細についてはこの種の配置後のスクリプトを使用して作業を自動化することで見つかります[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。

> [!NOTE]
> サーバーのプロジェクトおよびデータベースの詳細については、次を参照してください。 [Visual Studio 2010 の SQL Server データベース プロジェクト](https://msdn.microsoft.com/library/ff678491.aspx)です。


## <a name="configure-permissions-for-the-deployment-account"></a>展開アカウントのアクセス許可を構成します。

展開を実行するときに使用するアカウントが SQL Server の管理者でない場合は、このアカウントのログインを作成する必要ありますも。 アカウント データベースを作成するためのメンバーである必要があります、 **dbcreator**サーバーの役割と同等の権限を持っているか。

> [!NOTE]
> データベースを配置する Web デプロイまたは VSDBCMD を使用する場合は、(SQL Server のインスタンスが混合モード認証をサポートするために構成されている) 場合、Windows 資格情報または SQL Server 資格情報を使用できます。 次の手順には、Windows 資格情報を使用するが、展開を構成するときに、接続文字列での SQL Server のユーザー名とパスワードの指定から停止する nothing を使用する必要があることが前提とします。


**展開アカウントのアクセス許可を設定するには**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**  ウィンドウを右クリックして**セキュリティ**、 をポイント**新規**、クリックして**ログイン**です。
3. **ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、展開アカウントの名前を入力 (たとえば、 **FABRIKAM\matt**)。
4. **ページの選択** ウィンドウで、をクリックして**サーバーの役割**です。
5. 選択**dbcreator**、順にクリック**OK**です。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

後続のデプロイをサポートするためにもする必要がありますに展開するアカウントを追加、 **db\_所有者**最初の展開後に、データベースのロール。 後続のデプロイで、既存のデータベースのスキーマの変更はなくする新しいデータベースを作成するためです。 前のセクションで説明した、明確な理由により、データベースを作成するまで、データベース ロールにユーザーを追加することはできません。

**データベースに展開アカウントのログインをマップする\_データベース ロールの所有者**

1. 以前と同様、SQL Server Management Studio を開きます。
2. **オブジェクト エクスプ ローラー**ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックし、(たとえば、 **FABRIKAM\matt**)。
3. **ログイン プロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**です。
4. **このログインにマップされたユーザー**テーブルで、データベースの名前を選択 (たとえば、 **ContactManager**)。
5. **データベース ロールのメンバーシップ:** *[データベース名]*一覧で、、 **db\_所有者**ロール。

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. **[OK]**をクリックします。

## <a name="conclusion"></a>まとめ

データベース サーバーをリモート データベースの配置をそのまま使用して、データベースにアクセスするリモートの IIS web サーバーを許可する準備ができてはずです。 展開してデータベースを使用しようとすると、前にこれらの要点をチェックすることがあります。

- リモートの TCP/IP 接続を受け入れるように SQL Server を構成しましたか。
- SQL Server トラフィックを許可するようにファイアウォールを構成しましたか。
- SQL Server にアクセスするすべての web サーバーのマシン アカウントのログインを作成しましたか。
- データベースの配置は、ユーザー ロールのマッピングを作成するためのスクリプトを含めますが最初にデータベースを配置した後に手動で作成これらする必要がありますか。
- 展開アカウントのログインを作成し、追加してに、 **dbcreator**サーバーの役割ですか?

## <a name="further-reading"></a>関連項目

データベース プロジェクトを配置する方法については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。 配置後スクリプトを実行してデータベース ロールのメンバーシップを作成する方法のガイダンスについては、次を参照してください。[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。 メンバーシップ データベースがもたらされる独自の展開の課題に対応する方法のガイダンスについては、次を参照してください。[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)です。

> [!div class="step-by-step"]
> [前へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [次へ](creating-a-server-farm-with-the-web-farm-framework.md)
