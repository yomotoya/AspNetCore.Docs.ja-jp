---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: ターゲット環境の展開のプロパティの構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、連絡先のマネージャーのサンプル ソリューションを特定のターゲット環境に展開するために環境固有のプロパティを構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: ae9e220d1284bcc3aefed09a3f64cceac604fb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881589"
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>ターゲット環境の展開のプロパティを構成します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、連絡先のマネージャーのサンプル ソリューションを特定のターゲット環境に展開するために環境固有のプロパティを構成する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューション&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="process-overview"></a>プロセスの概要

ビルドし、連絡先のマネージャー ソリューションの配置に使用するプロジェクト ファイルは、2 つの物理ファイルに分かれています。

- ユニバーサルが含まれているビルド設定と指示 (、 *Publish.proj*ファイル)。
- ビルド設定のいずれかの特定環境にはが含まれています (*Env Dev.proj*、 *Env Stage.proj*など)。

ビルド時に適切な環境に固有のプロジェクト ファイルが、汎用にマージ*Publish.proj*ビルド手順の完全なセットを形成するファイル。 作成するか、独自の展開シナリオを説明する設定と環境固有のプロジェクト ファイルをカスタマイズする、特定の展開先環境への展開を構成できます。

これらの値の多くは、送信先の環境を構成する方法によって決まります&#x2014;Web Deployment Agent サービス (リモート エージェント) または Web デプロイ ハンドラーを使用する、ターゲット web サーバーが構成されているかどうかに具体的には、します。 これらの方法の詳細については、ご使用の環境にとって適切なアプローチを選択するためのガイダンスについては、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。

[連絡先のマネージャーのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)2 つの環境に固有のプロジェクト ファイルが必要です。

- 開発者のテスト環境に展開 (*Env Dev.proj*)。 開発者のテスト環境が」の説明に従って、リモート エージェントを使用してリモート展開を受け入れるように構成されている[シナリオ: Web 配置用のテスト環境構成](scenario-configuring-a-test-environment-for-web-deployment.md)です。 このファイルは、エンドポイントのアドレスだけでなく接続文字列などのサービス エンドポイントの場所に固有の設定は、リモート エージェントを提供する必要があります。
- ステージング環境にデプロイ (*Env Stage.proj*)。 ステージング環境が」の説明に従って、Web の展開のハンドラーを使用してリモート展開を受け入れるように構成されている[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。 このファイルは、Web 配置ハンドラー エンドポイント アドレスと接続文字列のような場所に固有の設定を提供し、サービス エンドポイントに必要です。

Web パッケージ自体の内容の環境に固有のプロジェクト ファイルで構成した設定に影響しないことを確認することが重要&#x2014;、制御、パッケージを展開する方法と、パッケージの場合にどのようなパラメーター値が提供される代わりに、抽出されます。 」の説明に従って、手動で web パッケージを実稼働環境にインポートしている[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)と[Web パッケージを手動でインストールする](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)、どのような設定にかかわらずで使用した環境固有のプロジェクト ファイル、パッケージを生成したときにします。 パッケージをインポートするときに、接続文字列と、サービス エンドポイントなど、任意のパラメーター化された値を要求インターネット インフォメーション サービス (IIS) マネージャーが表示されます。

に独自のターゲット環境を、連絡先のマネージャー ソリューションを配置するには、か、このファイルをカスタマイズまたはテンプレートとして使用でき、独自のファイルを作成できます。

**連絡先のマネージャー ソリューションの特定の環境のデプロイ設定を構成するには**

1. Visual Studio 2010 で ContactManager WCF ソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、展開、**発行**フォルダーで、展開、 **EnvConfig**フォルダー、およびをダブルクリック**Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. 内のプロパティ値を置き換える、 *Env Dev.proj*テスト環境の適切な値を持つファイルです。

    > [!NOTE]
    > この手順を次の表は、これらの各プロパティの詳細を提供します。
4. 作業内容を保存し、閉じます、 *Env Dev.proj*ファイル。

## <a name="choosing-the-right-deployment-properties"></a>適切な展開のプロパティ を選択

次の表は、サンプルの環境に固有のプロジェクト ファイル内の各プロパティの目的を説明*Env Dev.proj*と指定する必要があります値をいくつかのガイダンスを提供します。


|                                                        プロパティ名                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong>先の web サーバーまたはサービス エンドポイントの名前。               |                                                                                                                                                                                                                                              を、ターゲット web サーバー上のリモート エージェント サービスに配置する場合は、対象コンピューターの名前を指定できます (たとえば、 <strong>TESTWEB1</strong>または<strong>TESTWEB1.fabrikam.net</strong>)、またはリモートを指定することができますエージェントのエンドポイント (たとえば、 `http://TESTWEB1/MSDEPLOYAGENTSERVICE`)。 各ケースで同じように動作して展開します。 を、Web 展開ハンドラーで、ターゲット web サーバー上に配置する場合、サービス エンドポイントを指定し、クエリ文字列パラメーターとして、IIS web サイトの名前を含めます (たとえば、 `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`)。                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> Web Deploy がリモート コンピューターへの認証に使用するメソッド。          |                                                                                                                                                                                                                          これに設定する必要があります<strong>NTLM</strong>または<strong>基本</strong>です。 通常、使用する<strong>NTLM</strong>リモート エージェント サービスを配置する場合と<strong>基本的な</strong>Web 展開ハンドラーを配置する場合。 基本認証を使用する場合は、ユーザー名とパスワード (Web 配置) IIS Web 配置ツールの展開を実行するために権限を借用する必要がありますを指定する必要があります。 この例では、これらの値がによって提供される、 <strong>MSDeployUsername</strong>と<strong>MSDeployPassword</strong>プロパティです。 NTLM 認証を使用する場合は、これらのプロパティを省略するか、空白のままにします。                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong>基本認証を使用する場合は、Web Deploy はこのアカウントを使用、リモート コンピューター上です。  |                                                                                                                                                                                                                                                                                                                                                                                                                       これは、フォームにかける<em>ドメイン</em>\*username * (たとえば、 <strong>FABRIKAM\matt</strong>)。 この値は、基本認証を指定する場合にのみ使用します。 NTLM 認証を使用する場合、プロパティを省略できます。 値を指定する場合は無視されます。                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong>基本認証を使用する場合は、Web Deploy はパスワードを使ってこの、リモート コンピューター上です。 |                                                                                                                                                                                                                                                                                                                                                                                                                    これで指定したユーザー アカウントのパスワード、 <strong>MSDeployUsername</strong>プロパティです。 この値は、基本認証を指定する場合にのみ使用します。 NTLM 認証を使用する場合、プロパティを省略できます。 値を指定する場合は無視されます。                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong>連絡先のマネージャーの MVC アプリケーションを展開する、IIS パス。     |                                                                                                                                                                                                                                                                                                                                                                        これは、パスになります、IIS マネージャーで、フォームに表示される [<em>IIS web サイト名</em>]/[<em>web</em><em>アプリケーション名</em>] です。 IIS web サイトが、アプリケーションを展開する前に存在する必要があることに注意してください。 たとえば、DemoSite という IIS の web サイトを作成してある場合は、DemoSite/ContactManager として MVC アプリケーション用 IIS パスを指定可能性があります。                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong>連絡先のマネージャーの WCF サービスを展開する、IIS パス。    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          たとえば、DemoSite という IIS の web サイトを作成した場合は、として WCF サービスの IIS パスを指定でした<strong>DemoSite/ContactManagerService</strong>です。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong>を WCF サービスに到達できる URL。                   |                                                                                                                                                     これには、フォームをかかります [<em>IIS web サイトのルート URL</em>]/[<em>サービス アプリケーションの名前</em>]/[<em>サービス エンドポイント</em>]。 たとえば、IIS の web サイトをポート 85 で作成した場合、URL 夥しいフォーム`http://localhost:85/ContactManagerService/ContactService.svc`です。 同じサーバーに、MVC アプリケーションと WCF サービスが展開されていることに注意してください。 その結果、この URL がインストールされているコンピューターからアクセスだけです。 このため、URL で localhost または IP アドレスではなく、コンピューター名またはホスト ヘッダーを使用することをお勧めします。 コンピューター名またはホスト ヘッダーを使用する場合、[ループバック チェック](https://go.microsoft.com/?linkid=9805131)IIS のセキュリティ機能が、URL をブロックし、返す可能性があります、 <strong>HTTP 401.1 - 権限がありません</strong>エラーです。                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong>データベース サーバーの接続文字列。                  | 接続文字列では、両方の VSDBCMD に使う資格情報をデータベース サーバーに接続し、データベースと web サーバーのアプリケーション プールが、データベース サーバーに接続し、データベースとの対話に使用する資格情報の作成を決定します。 基本的には、ここにある 2 つの選択肢です。 指定できます<strong>統合セキュリティ = true</strong>、統合 Windows 認証を使用する場合:<strong>データ ソース = TESTDB1; Integrated Security = true</strong>を使用して、データベースを作成、ここでは、VSDBCMD 実行可能ファイルを実行しているユーザーおよびアプリケーションの資格情報は、web サーバーのコンピューター アカウントの id を使用してデータベースにアクセスされます。 または、ユーザー名と SQL Server アカウントのパスワードを指定することができます。 VSDBCMD データベースを作成して、データベースとやり取りするアプリケーション プールの両方に、ここでは、SQL Server 資格情報が使用される:<strong>データ ソース = TESTDB1 です。ユーザー Id = ASqlUser です。パスワード = Pa$ $w0rd</strong>のチュートリアルでは、このトピックでは、統合 Windows 認証を使用することを前提としています。 |
|        <strong>CmTargetDatabase</strong>データベース サーバーで作成するデータベースを提供する名前。        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     ここで指定する値は、パラメーターとして VSDBCMD コマンドに追加されます。 Web サーバー上のアプリケーション プールは、データベースとの対話に使用できる完全な接続文字列を作成することも使用されます。                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

これらの例では、特定の展開シナリオのこれらのプロパティを構成する方法を示しています。

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>例 1&#x2014;リモート エージェント サービスへの展開

この例では、次のように記述されています。

- TESTWEB1 上のリモート エージェント サービスを配置します。
- Web デプロイの NTLM 認証を使用することを指示するしています。 Web Deploy は、Microsoft Build Engine (MSBuild) を呼び出すために使用する資格情報を使用して実行されます。
- 展開する統合認証を使用している、 **ContactManager** TESTDB1 をデータベースします。 データベースは、MSBuild を呼び出すために使用する資格情報を使って展開されます。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>例 2&#x2014;Web への展開は、ハンドラーのエンドポイントを展開

この例では、次のように記述されています。

- STAGEWEB1 の Web 展開ハンドラーのサービス エンドポイントに配置します。
- Web デプロイの基本認証を使用することを指示するしています。
- Web 配置でリモート コンピューターで FABRIKAM\stagingdeployer アカウントを借用する必要があることを指定します。
- 展開する SQL Server 認証を使用している、 **ContactManager** STAGEDB1 へのデータベースです。


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>まとめ

この時点で、プロジェクト ファイルは完全をビルドし、1 つまたは複数の展開先環境への連絡先のマネージャー ソリューションの配置構成します。

をシングル ステップ、反復可能な展開プロセスの一環としてこれらのプロジェクト ファイルを使用するのには、を実行する必要があります、 *Publish.proj* MSBuild を使用してファイルし、環境固有のプロジェクト ファイルの場所をパラメーターとして渡します。 これは、さまざまな方法で行うことができます。

- MSBuild とカスタム プロジェクト ファイルの概要についての概要については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。
- カスタム プロジェクト ファイルを実行する MSBuild コマンドを作成する方法については、次を参照してください。 [Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。
- シングル ステップで反復可能な展開でコマンド ファイルに、MSBuild コマンドを組み込む方法については、次を参照してください。[の作成と展開コマンド ファイルを実行している](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)です。
- チーム ビルドからのカスタム プロジェクトのファイルを実行する方法については、次を参照してください。[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](creating-a-server-farm-with-the-web-farm-framework.md)
