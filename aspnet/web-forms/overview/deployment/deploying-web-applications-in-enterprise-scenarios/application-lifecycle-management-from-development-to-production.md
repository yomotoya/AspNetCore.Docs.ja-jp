---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'アプリケーション ライフ サイクル管理: 開発運用から |Microsoft Docs'
author: jrjlee
description: このトピックでは、架空の会社が par としてテスト、ステージング、実稼働環境での ASP.NET web アプリケーションの展開を管理する方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 47af9504bdef294b987cdd23ab1bcefbeadd4681
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808980"
---
<a name="application-lifecycle-management-from-development-to-production"></a>アプリケーション ライフ サイクル管理: 開発運用環境から
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、架空の会社がテスト、ステージング、実稼働環境で継続的な開発プロセスの一環として、ASP.NET web アプリケーションの展開を管理する方法を示しています。 トピック全体では、リンクは、詳しい情報と特定のタスクを実行する方法のチュートリアルに提供されます。
> 
> トピックがの概要を説明するように設計、[一連のチュートリアル](deploying-web-applications-in-enterprise-scenarios.md)企業の web 展開でします。 ここで説明した概念の一部に詳しくない場合に心配&#x2014;以下のチュートリアルは、これらのタスクと手法のすべての詳細情報を提供します。
> 
> > [!NOTE]
> > このためにわかりやすくするため、このトピックについては説明しませんデータベースの更新、展開プロセスの一部として。 ただし、多くのエンタープライズ展開シナリオの要件です。 データベースの機能に対して増分更新を行うと、このチュートリアル シリーズの後半でこれを実行する方法のガイダンスを見つけることができます。 詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。


## <a name="overview"></a>概要

ここで説明する展開プロセスが Fabrikam, Inc. の配置シナリオでに基づいて[エンタープライズ Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)します。 このトピックを調査する前に、シナリオの概要を確認してください。 基本的には、シナリオは組織が、かなり複雑な web アプリケーションの展開を管理する方法を調べ、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、一般的なエンタープライズ環境でさまざまなフェーズをします。

大まかに言えば、連絡先マネージャー ソリューションは、開発と展開プロセスの一部としてこれらのステージ。

1. 開発者は、Team Foundation Server (TFS) 2010 にいくつかのコードを確認します。
2. TFS は、コードをビルドし、チーム プロジェクトに関連付けられた任意の単体テストを実行します。
3. TFS では、テスト環境にソリューションをデプロイします。
4. 開発者チームは、ことを確認し、テスト環境でソリューションを検証します。
5. ステージング環境の管理者は、展開が、問題を引き起こすかどうかを確立するために、ステージング環境に"what if"展開を実行します。
6. ステージング環境の管理者は、ステージング環境に実際の展開を実行します。
7. ソリューションでは、ユーザー受け入れテスト、ステージング環境で行われます。
8. Web 展開パッケージは、運用環境に手動でインポートされます。

これらのステージでは、継続的な開発サイクルの一部を形成します。

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

実際には、さらに詳しくは、各ステージに注目するとわかるプロセスは、これよりも少し複雑になります。 Fabrikam, Inc. は、各ターゲット環境のデプロイに別のアプローチを使用します。

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

このトピックの残りの部分には、この展開のライフ サイクルのこれらの主要段階が調べられます。

- **前提条件**: どの場所に、デプロイ ロジックを配置する前に、サーバー インフラストラクチャを構成する必要があります。
- **初期開発とデプロイ**: 初めてのソリューションを配置する前に実行する必要があります。
- **展開テスト**: パッケージ化し、開発者が新しいコードをチェックインする際にテスト環境にコンテンツを自動的にデプロイする方法。
- **ステージング環境に展開**: 特定の展開方法ビルド ステージング環境および"what-if"を実行する方法を展開する展開で問題が発生しないことを確認します。
- **運用環境に展開**: ネットワーク インフラストラクチャがリモートの展開を実行できない場合は、運用環境に web パッケージをインポートする方法。

## <a name="prerequisites"></a>必須コンポーネント

展開シナリオでは、最初のタスクでは、サーバー インフラストラクチャが、展開ツールと手法の要件を満たしていることを確認します。 この場合は、Fabrikam, Inc. には、このような場合は、そのサーバー インフラストラクチャが構成します。

- TFS は、チーム プロジェクト コレクションを含める、ビルド コント ローラー、およびビルド エージェントに構成されます。 参照してください[Web デプロイの自動化用の Team Foundation Server の構成](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)詳細についてはします。
- テスト環境が」の説明に従って、Web Deployment Agent サービス (、「リモート エージェント」) を使用してリモート展開を受け入れるように構成されている[シナリオ: Web 展開のテスト環境を構成する](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md)と[Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)します。
- ステージング環境が」の説明に従って、Web 配置ハンドラー エンドポイントを使用してリモート展開を受け入れるように構成されている[シナリオ: Web デプロイのステージング環境を構成する](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md)と[Web サーバーを構成します。web 配置発行 (Web 配置ハンドラー)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)します。
- 運用環境の構成」の説明に従って、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする管理者を許可する[シナリオ: Web 配置の運用環境の構成](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md)と[Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。

## <a name="initial-development-and-deployment"></a>初期開発とデプロイ

Fabrikam, Inc. の開発チームは、連絡先マネージャー ソリューションをデプロイ、最初に、これらのタスクを実行する必要があります。

- TFS で新しいチーム プロジェクトを作成します。
- デプロイ ロジックを含む Microsoft Build Engine (MSBuild) プロジェクト ファイルを作成します。
- 展開プロセスをトリガーする TFS ビルド定義を作成します。

### <a name="create-a-new-team-project"></a>新しいチーム プロジェクトを作成します。

- TFS 管理者は、Rob Walters、」の説明に従って、アプリケーションの新しいチーム プロジェクトを作成します[TFS でチーム プロジェクトを作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md)します。 次に、リード開発者、Matt 明では、スケルトンのソリューションを作成します。 TFS では、新しいチーム プロジェクトに自分のファイルをチェックしています」の説明に従って[ソース管理へのコンテンツを追加する](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md)します。

### <a name="create-the-deployment-logic"></a>デプロイ ロジックを作成します。

Matt の明を作成で説明されている分割プロジェクト ファイルの方法を使用してさまざまなカスタム MSBuild プロジェクト ファイル、[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。 Matt が作成されます。

- という名前のプロジェクト ファイル*Publish.proj*展開プロセスを実行します。 このファイルには、ソリューションでプロジェクトをビルド、web のパッケージを作成および変換先のサーバー環境にパッケージを配置する MSBuild ターゲットが含まれています。
- という名前の環境に固有のプロジェクト ファイル*Env Dev.proj*と*Env Stage.proj*します。 これらには、固有の設定、テスト環境とステージング環境にそれぞれ、接続文字列、サービス エンドポイント、および web パッケージを受信するリモート サービスの詳細などが含まれます。 特定の展開先環境の適切な設定を選択する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。

展開を実行するユーザーが実行される、 *Publish.proj* MSBuild またはチーム ビルドを使用してファイルし、関連する環境に固有のプロジェクト ファイルの場所を指定します (*Env Dev.proj*または*Env Stage.proj*) コマンドライン引数として。 *Publish.proj*ファイルからの発行手順については、各ターゲット環境の完全なセットを作成する環境固有のプロジェクト ファイルをインポートします。

> [!NOTE]
> これらのカスタム プロジェクト ファイルの作業方法は、メカニズムを使用して MSBuild を呼び出す依存しません。 たとえば、する MSBuild のコマンドラインを直接使用できます」の説明に従って[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。 」の説明に従ってから、コマンド ファイルをプロジェクト ファイルを実行することができます[を作成し、展開コマンド ファイルを実行](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)します。 または、行うことができます、プロジェクト ファイル TFS では、ビルド定義から」の説明に従って[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)します。  
> 最終的な結果では各ケースでは、同じ&#x2014;MSBuild がマージされたプロジェクト ファイルを実行し、ターゲット環境にソリューションをデプロイします。 これにより、発行のプロセスをトリガーする方法の柔軟性の大幅。


彼がカスタムのプロジェクト ファイルを作成して、Matt はソリューション フォルダーに追加し、ソース管理にチェックインします。

### <a name="create-build-definitions"></a>ビルド定義を作成します。

最後の準備タスクとして Matt と Rob 連携新しいチーム プロジェクトの 3 つのビルド定義を作成します。

- **DeployToTest**します。 連絡先マネージャー ソリューションがビルドされ、チェックインが発生するたびにテスト環境にデプロイします。
- **DeployToStaging**します。 これにより、開発者は、ビルドをキューときに、ステージング環境に、指定された以前のビルドからのリソースをデプロイします。
- **DeployToStaging-whatif**します。 これにより、開発者は、ビルドをキューときに、ステージング環境に"what if"展開を実行します。

次のセクションでは、これらの詳細を説明するビルド定義。

## <a name="deployment-to-test"></a>Test へのデプロイ

Fabrikam, Inc. の開発チームは、さまざまなソフトウェア テストを検証し、検証、ユーザビリティ テスト、互換性のテスト、およびアドホックまたは探索的テストなどのアクティビティを実行するテスト環境を維持します。

開発チームがという名前の TFS でビルド定義を作成した**DeployToTest**します。 このビルド定義では、Fabrikam, Inc. の開発チームのメンバーがチェックインを実行するたびに、ビルド プロセスが実行されることを意味、継続的インテグレーションのトリガーを使用します。 ビルドがトリガーされたときに、ビルド定義を行います。

- ContactManager.sln ソリューションをビルドします。 さらに、ソリューション内のすべてのプロジェクトがビルドされます。
- (ソリューションが正常にビルド) の場合は、ソリューション フォルダーの構造で、単体テストを実行します。
- (ソリューションが正常にビルドされ、任意の単体テストに合格) 場合は、展開プロセスを制御するカスタム プロジェクト ファイルを実行します。

最終的な結果は、ソリューションが正常にビルド、単体テストを渡す場合は、web パッケージとその他の配置リソースがデプロイされるテスト環境にします。

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

**DeployToTest**定義装置を MSBuild にこれらの引数をビルドします。


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true**と**DeployTarget パッケージ =** プロパティは、ソリューション内のプロジェクトのビルド チーム ビルド時に使用されます。 Web アプリケーション プロジェクトをプロジェクトには、これらのプロパティは、プロジェクトの web 配置パッケージを作成するために MSBuild を指示します。 **TargetEnvPropsFile**プロパティは、通知、 *Publish.proj*ファイルをインポートする環境に固有のプロジェクト ファイルを検索します。

> [!NOTE]
> このようなビルド定義を作成する方法の詳細なチュートリアルについてを参照してください。[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)します。


*Publish.proj*ファイルには、ソリューション内の各プロジェクトをビルドするターゲットが含まれています。 ただしをスキップしますこれらのターゲットがビルド チーム ビルドで、ファイルを実行している場合は、条件付きロジックも含まれています。 単体テストを実行する機能など、提供チームがビルドされる追加のビルドの機能を活用できます。 ソリューションのビルドまたは単体テストに、失敗した場合、 *Publish.proj*ファイルは実行しないと、アプリケーションは展開されません。

条件付きロジックを評価することによって実現されます、 **BuildingInTeamBuild**プロパティ。 これは MSBuild のプロパティに自動的に設定されている**true**とするチーム ビルドを使用して、プロジェクトをビルドします。

## <a name="deployment-to-staging"></a>ステージングへのデプロイ

ビルドでは、すべてのテスト環境での開発者チームの要件を満たして、ときにチームが同じビルドをステージング環境にデプロイするとします。 ステージング環境は通常、実稼働可能な限り密接に、たとえば、サーバーの仕様、オペレーティング システムとソフトウェア、およびネットワークの構成の観点から「ライブ」の環境としての特性に一致するように構成します。 ステージング環境は、ロード テスト、ユーザー受け入れテスト、およびより広範な社内レビューのよく使用されます。 ビルドは、ビルド サーバーから直接ステージング環境にデプロイされます。

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

ステージング環境にソリューションをデプロイするために使用するビルド定義**DeployToStaging-whatif**と**DeployToStaging**、これらの特性を共有します。

- 何も実際に構築しません。 Rob でソリューションをステージング環境に展開すると、彼が既に確認され、テスト環境で検証する、特定の既存のビルドをデプロイします。 ビルド定義は、展開プロセスを制御するカスタムのプロジェクト ファイルを実行するだけです。
- Rob ビルドがトリガーされるとビルド パラメーターを使用しています、ビルドには、ビルド サーバーから展開するリソースが含まれています。 を指定します。
- ビルド定義は自動的にトリガーされません。 Rob は、ステージング環境にソリューションをデプロイするとき、ビルドを手動でキューします。

これは、ステージング環境にデプロイする手順の概要について説明します。

1. ステージング環境管理者、Rob Walters、キューを使用して、ビルド、 **DeployToStaging-whatif**定義を作成します。 Rob は、ビルド定義のパラメーターを使用し、展開するビルドを指定します。
2. **DeployToStaging-whatif**定義の実行に"what if"モードでのカスタムのプロジェクト ファイルをビルドします。 Rob は実際の展開を実行していたが、送信先の環境に、変更を加えることは実際には、ログ ファイルが生成されます。
3. Rob は、ステージング環境で、展開の影響を確認するためにログ ファイルを確認します。 具体的には、Rob は何が追加されます、何も更新されます、および削除対象を確認しようとします。
4. Rob の場合を使用して、ビルドはキュー、配置が既存のリソースまたはデータに望ましくない変更を加えるしないことを満たす場合、 **DeployToStaging**定義を作成します。
5. **DeployToStaging**定義の実行、カスタムのプロジェクト ファイルをビルドします。 これらの展開リソース、ステージング環境でプライマリ web サーバーに発行します。
6. Web Farm Framework (WFF) コント ローラーは、ステージング環境で web サーバーを同期します。 これにより、サーバー ファーム内のすべての web サーバー上で、アプリケーションが使用可能なにします。

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

**DeployToStaging**定義装置を MSBuild にこれらの引数をビルドします。


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile**プロパティは、通知、 *Publish.proj*ファイルをインポートする環境に固有のプロジェクト ファイルを検索します。 **OutputRoot**プロパティは、組み込みの値をオーバーライドし、デプロイするリソースが含まれるビルド フォルダーの場所を示します。 Rob は、ビルドをキュー、ときに使用して、**パラメーター**  タブの更新後の値を提供する、 **OutputRoot**プロパティ。

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> このようなビルド定義を作成する方法の詳細については、次を参照してください。[特定のビルドをデプロイ](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md)します。


**DeployToStaging-whatif**ビルド定義にはと同じ配置ロジックが含まれています、 **DeployToStaging**定義を作成します。 ただし、追加の引数を含めて**WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


内で、 *Publish.proj*ファイル、 **WhatIf**プロパティは、すべての配置リソースが"what-if"モードで公開することを示します。 つまり、展開にそうなったが、送信先の環境で何も変更は実際には、ログ ファイルが生成されます。 これにより、提案された展開の影響を評価できます&#x2014;で特定の内容は、追加、取得とは、更新、および削除はどのような&#x2014;実際に変更を加える前にします。

> [!NOTE]
> "What if"展開を構成する方法の詳細については、次を参照してください。 ["What If"展開を実行する](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md)します。


ステージング環境でプライマリ web サーバーにアプリケーションをデプロイすると、WFF は自動的に同期されているサーバー ファーム内のすべてのサーバー間でアプリケーションを使用します。

> [!NOTE]
> Web サーバーを同期する WFF の構成の詳細については、次を参照してください。 [Web Farm Framework でサーバー ファームを作成する](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md)します。


## <a name="deployment-to-production"></a>運用環境へのデプロイ

ビルドはステージング環境で承認されると、Fabrikam, Inc. チームは運用環境にアプリケーションを発行できます。 運用環境は、アプリケーションが「ライブ」に移動し、エンドユーザーの対象ユーザーに達するとは。

運用環境では、インターネットに接続する境界ネットワーク内です。 これは、ビルド サーバーが含まれている内部ネットワークから分離されます。 Lisa Andrews、運用環境の管理者は、ビルド サーバーからの web 展開パッケージをコピーし、プライマリ実稼働 web サーバーで IIS にインポートする必要があります手動で。

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

これは、運用環境にデプロイする手順の概要について説明します。

1. 開発者チーム ビルドが実稼働環境にデプロイできる状態である Lisa が表示されます。 チームでは、ビルド サーバーで、ドロップ フォルダー内の web 展開パッケージの場所の Lisa が表示されます。
2. Lisa は、ビルド サーバーから web パッケージを収集し、運用環境でプライマリ web サーバーにコピーします。
3. Lisa では、IIS マネージャーを使用して、インポートして、プライマリ web サーバー上の web パッケージを発行します。
4. WFF コント ローラーは、運用環境で web サーバーを同期します。 これにより、サーバー ファーム内のすべての web サーバー上で、アプリケーションが使用可能なにします。

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

IIS マネージャーには、IIS の web サイトに web パッケージを公開しやすくインポート アプリケーション パッケージのウィザードが含まれています。 この手順を実行する方法のチュートリアルは、次を参照してください。 [Web パッケージを手動でインストール](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)します。

## <a name="conclusion"></a>まとめ

このトピックでは、一般的なエンタープライズ規模の web アプリケーションの展開のライフ サイクルの図が提供されています。

このトピックでは、一連の web アプリケーションのデプロイのさまざまな側面に関するガイダンスを提供するチュートリアルの一部を形成します。 実際には、展開プロセスの各段階で追加のタスクおよび考慮事項の多くがあるし、それらをすべて 1 つのチュートリアルでカバーすることはできません。 詳細については、これらのチュートリアルを参照してください。

- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 このチュートリアルでは、MSBuild と IIS の Web 配置ツール (Web 配置) を使用して web 配置技術の包括的な概要を提供します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。 このチュートリアルでは、さまざまなデプロイ シナリオをサポートするために Windows server 環境を構成する方法のガイダンスを提供します。
- [Web 配置の自動化の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 このチュートリアルでは、デプロイ ロジックを TFS のビルド プロセスに統合する方法に関するガイダンスを提供します。
- [エンタープライズ Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、組織が直面するより複雑な展開の課題の一部を満たすについてガイダンスを説明します。

> [!div class="step-by-step"]
> [前へ](enterprise-web-deployment-scenario-overview.md)
