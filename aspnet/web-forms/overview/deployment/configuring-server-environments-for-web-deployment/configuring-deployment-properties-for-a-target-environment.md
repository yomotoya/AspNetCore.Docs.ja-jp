---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: ターゲット環境の展開のプロパティの構成 |Microsoft Docs
author: jrjlee
description: このトピックでは、特定のターゲット環境にサンプルの連絡先マネージャー ソリューションをデプロイするために環境固有のプロパティを構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: 8e0a050b2fec272fec3922d1c2f3afff4aee7fca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387687"
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>ターゲット環境の展開のプロパティを構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、特定のターゲット環境にサンプルの連絡先マネージャー ソリューションをデプロイするために環境固有のプロパティを構成する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューション&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="process-overview"></a>プロセスの概要

連絡先マネージャー ソリューション ビルドしてデプロイに使用するプロジェクト ファイルは、2 つの物理ファイルに分かれています。

- ユニバーサルを含む 1 つのビルド設定および処理手順 (、 *Publish.proj*ファイル)。
- 特定の環境を含む 1 つのビルド設定 (*Env Dev.proj*、 *Env Stage.proj*など)。

ビルド時に、適切な環境に固有のプロジェクト ファイルは、universal にマージ*Publish.proj*ビルド手順の完全なセットを形成するファイル。 作成または展開シナリオについて説明する設定と環境固有のプロジェクト ファイルをカスタマイズすることで、特定の展開先環境へのデプロイを構成できます。

これらの値の多くは、移行先環境を構成する方法によって決まります&#x2014;Web Deployment Agent サービス (リモート エージェント) または Web 配置ハンドラーを使用して、ターゲット web サーバーが構成されているかどうかに具体的には、します。 これらの方法の詳細については、および独自の環境の適切なアプローチの選択に関するガイダンスについては、「 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)します。

[Contact Manager シナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)2 つの環境に固有のプロジェクト ファイルが必要です。

- 開発者のテスト環境への配置 (*Env Dev.proj*)。 開発者のテスト環境が」の説明に従って、リモート エージェントを使用してリモート展開を受け入れるように構成されている[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。 このファイルは、エンドポイント アドレスだけでなく、接続文字列などのサービス エンドポイントの場所に固有の設定は、リモート エージェントを提供する必要があります。
- ステージング環境への配置 (*Env Stage.proj*)。 ステージング環境が」の説明に従って、Web 配置ハンドラーを使用してリモート展開を受け入れるように構成されている[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。 このファイルは、Web 配置ハンドラーのエンドポイント アドレスと接続文字列のような場所に固有の設定を提供し、エンドポイントをサービスする必要があります。

Web パッケージ自体の内容の環境に固有のプロジェクト ファイルで構成設定に影響しないことを確認することが重要&#x2014;制御代わりに、パッケージの展開方法と、どのようなパラメーター値は、パッケージが提供されます抽出されます。 」の説明に従って、手動で web パッケージを運用環境にインポートする[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)と[Web パッケージを手動でインストール](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)、設定かは関係ありませんので、環境固有のプロジェクト ファイルでときに使用したパッケージを生成します。 パッケージをインポートするときに、接続文字列と、サービス エンドポイントなどの任意のパラメーター化された値を要求インターネット インフォメーション サービス (IIS) マネージャーが表示されます。

連絡先マネージャー ソリューションを独自のターゲット環境をデプロイするには、か、このファイルをカスタマイズまたはこれをテンプレートとして使用でき、独自のファイルを作成できます。

**連絡先マネージャー ソリューションを環境に固有の展開設定を構成するには**

1. Visual Studio 2010 では、ContactManager WCF ソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、展開、**発行**フォルダー、展開、 **EnvConfig**フォルダー、およびダブルクリック**Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. プロパティ値を置き換える、 *Env Dev.proj*テスト環境の正しい値を持つファイル。

    > [!NOTE]
    > この手順を次の表では、これらの各プロパティの詳細を提供します。
4. 作業内容を保存し、閉じます、 *Env Dev.proj*ファイル。

## <a name="choosing-the-right-deployment-properties"></a>適切な展開のプロパティ を選択

この表は、サンプルの環境に固有のプロジェクト ファイル内の各プロパティの目的は、 *Env Dev.proj*を指定する必要があります値のいくつかの指針を示します。


|                                                        プロパティ名                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong>変換先の web サーバーまたはサービス エンドポイントの名前。               |                                                                                                                                                                                                                                              移行先の web サーバーでリモート エージェント サービスをデプロイする場合は、対象コンピューターの名前を指定できます (たとえば、 <strong>TESTWEB1</strong>または<strong>TESTWEB1.fabrikam.net</strong>)、またはリモートを指定できますエージェントのエンドポイント (たとえば、 `http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。 展開いずれの場合も同じように動作します。 移行先の web サーバーで Web 配置ハンドラーに配置する場合、サービス エンドポイントを指定し、クエリ文字列パラメーターとして、IIS web サイトの名前を含めます (たとえば、 `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> Web Deploy がリモート コンピューターへの認証に使用するメソッド。          |                                                                                                                                                                                                                          これに設定する必要があります<strong>NTLM</strong>または<strong>基本的な</strong>します。 通常、使用する<strong>NTLM</strong>リモート エージェント サービスをデプロイする場合と<strong>基本的な</strong>Web 配置ハンドラーに配置する場合。 基本認証を使用する場合は、ユーザー名とパスワード (Web 配置) IIS Web 配置ツールの配置を実行するために権限を借用する必要がありますを指定する必要があります。 この例ではこれらの値を通じて提供されます、 <strong>MSDeployUsername</strong>と<strong>MSDeployPassword</strong>プロパティ。 NTLM 認証を使用する場合は、これらのプロパティを省略するか、空白のままにできます。                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong>基本認証を使用する場合は、Web Deploy はこのアカウントを使用、リモート コンピューター上です。  |                                                                                                                                                                                                                                                                                                                                                                                                                       これは、形式をとる必要があります<em>ドメイン</em>\*ユーザー名 * (たとえば、 <strong>FABRIKAM\matt</strong>)。 この値は基本認証を指定する場合にのみ使用します。 NTLM 認証を使用する場合、プロパティを省略できます。 値が指定されている場合は、これは無視されます。                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong>基本認証を使用する場合 Web Deploy はパスワードを使用してこのリモート コンピューター。 |                                                                                                                                                                                                                                                                                                                                                                                                                    これで指定したユーザー アカウントのパスワード、 <strong>MSDeployUsername</strong>プロパティ。 この値は基本認証を指定する場合にのみ使用します。 NTLM 認証を使用する場合、プロパティを省略できます。 値が指定されている場合は、これは無視されます。                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong> Contact Manager MVC アプリケーションを配置する、IIS パス。     |                                                                                                                                                                                                                                                                                                                                                                        IIS マネージャーで、フォームに表示されるパスを指定する必要がありますこれ [<em>IIS web サイト名</em>]/[<em>web</em><em>アプリケーション名</em>]。 IIS web サイトが、アプリケーションを展開する前に存在する必要があることに注意してください。 たとえば、DemoSite という名前の IIS の web サイトを作成した場合は、DemoSite/ContactManager として MVC アプリケーション用の IIS パスを指定できます。                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong> Contact Manager WCF サービスをデプロイする、IIS パス。    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          たとえば、DemoSite という名前の IIS の web サイトを作成した場合は、として WCF サービスの IIS パスを指定でした<strong>DemoSite/ContactManagerService</strong>します。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong> WCF サービスに到達できる URL。                   |                                                                                                                                                     これは、形式になります [<em>IIS web サイトのルート URL</em>]/[<em>サービス アプリケーション名</em>]/[<em>サービス エンドポイント</em>]。 たとえばをポート 85 で IIS の web サイトを作成した場合、URL が具現化する`http://localhost:85/ContactManagerService/ContactService.svc`します。 MVC アプリケーションと WCF サービスが同じサーバーに展開されていることに注意してください。 その結果、この URL は、のみがインストールされているコンピューターからアクセスします。 このため、URL で localhost または、IP アドレスではなく、コンピューター名またはホスト ヘッダーを使用することをお勧めします。 コンピューター名またはホスト ヘッダーを使用する場合、[ループバック チェック](https://go.microsoft.com/?linkid=9805131)IIS でのセキュリティ機能が URL をブロックし、返す可能性があります、 <strong>HTTP 401.1 - 権限がありません</strong>エラー。                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong>データベース サーバーの接続文字列。                  | 接続文字列は、両方の資格情報が、VSDBCMD をデータベース サーバーに接続し、データベースと web サーバーのアプリケーション プールが、データベース サーバーに接続し、データベースとの対話に使用する資格情報の作成に使用するを決定します。 基本的がある 2 つの選択肢は、ここです。 指定できます<strong>Integrated Security = true</strong>、統合 Windows 認証を使用する場合:<strong>データ ソース = TESTDB1; Integrated Security = true</strong>を使用して、データベースを作成、ここでは、実行可能ファイル、VSDBCMD を実行しているユーザーおよびアプリケーションの資格情報は、web サーバーのコンピューター アカウントの id を使用してデータベースにアクセスされます。 ユーザー名と SQL Server アカウントのパスワードを指定することができます。 VSDBCMD は、データベースを作成して、データベースと対話するアプリケーション プールの両方に SQL Server 資格情報を使用するこの例では、:<strong>データ ソース = TESTDB1;ユーザー Id = ASqlUser;パスワード = Pa$ $w0rd</strong>のチュートリアルでは、このトピックでは、統合 Windows 認証を使用することを前提としています。 |
|        <strong>CmTargetDatabase</strong>名前に付ける、データベースのデータベース サーバーで作成します。        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     ここで指定する値は、VSDBCMD コマンドにパラメーターとして追加されます。 Web サーバー上のアプリケーション プールは、データベースとの対話に使用できる完全な接続文字列を作成することも使用されます。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

これらの例では、特定の展開シナリオのこれらのプロパティを構成する方法を示します。

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>例 1&#x2014;リモート エージェントのサービスへのデプロイ

この例では、次のように記述されています。

- TESTWEB1 でリモート エージェントのサービスにデプロイします。
- Web 配置で NTLM 認証を使用してを指示しています。 Web Deploy は、Microsoft Build Engine (MSBuild) を呼び出すために使用する資格情報を使用して実行されます。
- デプロイに統合認証を使用している、 **ContactManager**データベース TESTDB1 をします。 MSBuild を呼び出すために使用する資格情報を使用して、データベースが配置されます。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>例 2&#x2014;web 配置ハンドラーのエンドポイントをデプロイします。

この例では、次のように記述されています。

- STAGEWEB1 の Web 配置ハンドラーのサービス エンドポイントにデプロイします。
- Web 配置で基本認証を使用してを指示しています。
- Web 配置でリモート コンピューターで FABRIKAM\stagingdeployer アカウントを借用する必要があることを指定しています。
- デプロイには、SQL Server 認証を使用している、 **ContactManager** STAGEDB1 データベース。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>まとめ

この時点では、ビルドし、1 つまたは複数の展開先環境に連絡先マネージャー ソリューションをデプロイするプロジェクト ファイルが完全に構成します。

これらのプロジェクト ファイルを使用して、シングル ステップ、反復可能な展開プロセスの一環として、実行する必要があります、 *Publish.proj* MSBuild を使用してファイルし、環境固有のプロジェクト ファイルの場所をパラメーターとして渡します。 これは、さまざまな方法で行うことができます。

- MSBuild やカスタム プロジェクト ファイルの概要の概要については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。
- カスタムのプロジェクト ファイルを実行する MSBuild コマンドを考案する方法については、次を参照してください。 [Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)します。
- シングル ステップで反復可能な展開でコマンド ファイルに、MSBuild コマンドを組み込む方法については、次を参照してください。[の作成と展開コマンド ファイルを実行している](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)します。
- チーム ビルドから、カスタムのプロジェクト ファイルを実行する方法については、次を参照してください。[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](creating-a-server-farm-with-the-web-farm-framework.md)
