---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'アプリケーション ライフ サイクル管理: 実稼働環境に開発から |Microsoft ドキュメント'
author: jrjlee
description: このトピックでは、架空の会社が par としてテスト、ステージング、実稼働環境での ASP.NET web アプリケーションの展開を管理する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 8beeffb374df09c6695a1845199d30006ddcc1b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887286"
---
<a name="application-lifecycle-management-from-development-to-production"></a>アプリケーション ライフ サイクル管理: 実稼働環境に開発から
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、架空の会社がテスト、ステージング、実稼働環境で継続的な開発プロセスの一環として、ASP.NET web アプリケーションの展開を管理する方法について説明します。 トピック全体では、リンクは、詳しい情報と特定のタスクを実行する方法のチュートリアルに提供されます。
> 
> トピックの概要を説明するものでは、[一連のチュートリアル](deploying-web-applications-in-enterprise-scenarios.md)企業内の web 配置でします。 ここで説明する概念の一部をあまり詳しくない場合、心配しないで&#x2014;に従って、チュートリアルは、すべてこれらのタスクと手法の詳細情報を提供します。
> 
> > [!NOTE]
> > わかりやすくするためのために作成、このトピックはしない更新データベースを展開プロセスの一部として説明します。 ただし、多くのエンタープライズ展開シナリオの要件は、データベースの機能に対して増分更新を行うと、このチュートリアル シリーズの後でこれを実現する方法のガイダンスを見つけることができます。 詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。


## <a name="overview"></a>概要

ここで説明する展開プロセスが Fabrikam, Inc. の展開シナリオでに基づいて[Enterprise Web 展開: シナリオの概要](enterprise-web-deployment-scenario-overview.md)です。 このトピックの内容を調査する前に、シナリオの概要を読み込む必要があります。 基本的に、シナリオを調べ、組織が、ある程度複雑な web アプリケーションの展開を管理する方法、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、一般的なエンタープライズ環境でさまざまなフェーズがあります。

大まかに言えば、連絡先のマネージャー ソリューションの開発と展開プロセスの一環としてこれらのステージを通過します。

1. 開発者は、Team Foundation Server (TFS) 2010 にいくつかのコードを確認します。
2. TFS は、コードをビルドし、チーム プロジェクトに関連付けられている他の単体テストを実行します。
3. TFS は、テスト環境にソリューションを展開します。
4. 開発チームは、ことを確認し、テスト環境でソリューションを検証します。
5. ステージング環境の管理者では、展開が、問題を引き起こすかどうかを確立するために、ステージング環境への"what-if"展開を実行します。
6. ステージング環境の管理者では、ステージング環境へのライブの展開を実行します。
7. ソリューションでは、ユーザー受け入れテスト、ステージング環境で行われます。
8. Web 展開パッケージは、実稼働環境に手動でインポートされます。

これらの段階では、継続的な開発サイクルの一部を形成します。

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

実際には、プロセスは、さらに詳しくは、各段階で説明するよう、これよりもやや複雑です。 Fabrikam, Inc. は、各ターゲット環境のデプロイに別のアプローチを使用します。

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

このトピックの残りの部分には、この展開のライフ サイクルの各キーの段階が調べられます。

- **前提条件**: どの場所に、デプロイ ロジックを配置する前に、サーバー インフラストラクチャを構成する必要があります。
- **初期開発と展開**: 最初に、ソリューションを配置する前に実行する必要があります。
- **テストへの配置**: パッケージ化し、開発者は新しいコードでチェックするときに、テスト環境にコンテンツを自動的に展開する方法です。
- **ステージング展開**: 特定の展開方法では、展開で問題が発生しないことを確認してください。 への展開をステージング環境、"what-if"を実行する方法に構築します。
- **実稼働環境に展開**: ネットワーク インフラストラクチャにより、リモートの展開時に web パッケージを実稼働環境にインポートする方法です。

## <a name="prerequisites"></a>必須コンポーネント

どの展開シナリオの最初のタスクでは、サーバー インフラストラクチャが、展開ツールと手法の要件を満たしていることを確認します。 この例では、Fabrikam, Inc. は、次のように、サーバー インフラストラクチャが構成します。

- TFS では、チーム プロジェクト コレクションが含まれます、ビルド コント ローラー、およびビルド エージェントが構成されます。 参照してください[Web 配置の自動化の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)詳細についてはします。
- テスト環境が」の説明に従って、Web Deployment Agent サービス (「リモート エージェント」) を使用してリモート展開を受け入れるように構成されている[シナリオ: Web 配置用のテスト環境構成](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md)と[Web デプロイの発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。
- ステージング環境が」の説明に従って、Web 配置ハンドラー エンドポイントを使用してリモート展開を受け入れるように構成されている[シナリオ: Web 配置用のステージング環境構成](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md)と[Web サーバーを構成します。Web デプロイの発行 (Web Deploy ハンドラー)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。
- 実稼働環境の構成」の説明に従って、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする管理者を許可する[シナリオ: Web 配置の実稼働環境の構成](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md)と[Web デプロイの発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)です。

## <a name="initial-development-and-deployment"></a>初期開発と展開

Fabrikam, Inc. の開発チームは、最初に連絡先のマネージャー ソリューションを配置することができます、前にこれらのタスクを実行する必要があります。

- TFS で、新しいチーム プロジェクトを作成します。
- 配置ロジックを含む Microsoft Build Engine (MSBuild) プロジェクト ファイルを作成します。
- 展開プロセスを開始する TFS ビルド定義を作成します。

### <a name="create-a-new-team-project"></a>新しいチーム プロジェクトを作成します。

- TFS 管理者は、Rob Walters を」の説明に従って、アプリケーションの新しいチーム プロジェクトを作成[TFS でチーム プロジェクトの作成](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md)です。 次に、リード開発者、Matt 明では、スケルトンのソリューションを作成します。 」の説明に従って、TFS では、新しいチーム プロジェクトに自分のファイルをチェックイン彼[ソース管理へのコンテンツを追加する](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md)です。

### <a name="create-the-deployment-logic"></a>デプロイ ロジックを作成します。

Matt 明さまざまなカスタム MSBuild プロジェクト ファイルが作成で説明されている分割プロジェクト ファイルのアプローチを使用して[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。 Matt を作成します。

- という名前のプロジェクト ファイル*Publish.proj*展開プロセスを実行します。 このファイルには、ソリューションでプロジェクトをビルド、web のパッケージを作成および変換先のサーバー環境にパッケージを配置する MSBuild ターゲットが含まれています。
- という名前の環境に固有のプロジェクト ファイル*Env Dev.proj*と*Env Stage.proj*です。 固有の設定、テスト環境とステージング環境にそれぞれ、接続文字列、サービス エンドポイント、および web パッケージを受信するリモート サービスの詳細などが含まれます。 特定の送信先の環境に適切な設定を選択する方法の詳細については、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。

ユーザーが実行するには、展開を実行、 *Publish.proj* MSBuild またはチーム ビルドを使用してファイルし、関連する環境に固有のプロジェクト ファイルの場所を指定します (*Env Dev.proj*または*Env Stage.proj*)、コマンドライン引数。 *Publish.proj*ファイルは、発行については、各ターゲット環境の完全なセットを作成する環境に固有のプロジェクト ファイルをインポートします。

> [!NOTE]
> これらのカスタム プロジェクト ファイルの作業方法を使用して MSBuild を呼び出すメカニズムに依存しないです。 たとえば、ことができます、MSBuild コマンドラインを直接使用する、」の説明に従って[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。 」の説明に従って、コマンド ファイルから、プロジェクト ファイルを実行することができます[を作成および展開コマンド ファイルを実行](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md)です。 または、ファイルを実行できます、プロジェクトを TFS では、ビルド定義から」の説明に従って[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)です。  
> 最終結果では各ケースでは同じ&#x2014;MSBuild がマージされたプロジェクト ファイルを実行し、ソリューションは、ターゲット環境に配置します。 これにより、発行プロセスをトリガーする方法の柔軟性が大幅に向上します。


カスタム プロジェクト ファイルが作成した、Matt はソリューション フォルダーにそれらを追加し、ソース管理にチェックインします。

### <a name="create-build-definitions"></a>ビルド定義を作成します。

最後の準備タスクとして Matt と Rob 連携により、新しいチーム プロジェクトの次の 3 つのビルド定義を作成します。

- **DeployToTest**です。 連絡先のマネージャー ソリューションがビルドされ、チェックインが発生するたびに、テスト環境に展開します。
- **DeployToStaging**です。 これにより、開発者がビルドをキューときに、ステージング環境に指定された以前のビルドからのリソースが配置します。
- **DeployToStaging-whatif**です。 開発者は、ビルドをキューときに、ステージング環境に"what-if"デプロイを実行します。

以降のセクションで詳しく説明でこれらの各ビルド定義をビルドします。

## <a name="deployment-to-test"></a>テストへの展開

Fabrikam, Inc. の開発チームは、さまざまなソフトウェア テストを検証し、検証、ユーザビリティ テスト、互換性テスト、およびアドホックまたは探索的テストなどのアクティビティを実行するためにテスト環境を維持します。

開発チームは TFS という名前のビルド定義を作成するが**DeployToTest**です。 このビルド定義は、Fabrikam, Inc. の開発チームのメンバーがチェックインを実行するたびに、ビルド プロセスが実行されることを意味する、継続的インテグレーション トリガーを使用します。 ビルド定義は、ビルドがトリガーされたときに行います。

- ContactManager.sln ソリューションをビルドします。 これは、さらに、ソリューション内のすべてのプロジェクトをビルドします。
- (ソリューションで正常にビルドされる) 場合は、ソリューション フォルダーの構造で他の単体テストを実行します。
- (ソリューションが正常にビルドされ、任意の単体テストに合格した) 場合は、展開プロセスを制御するカスタム プロジェクト ファイルを実行します。

最終的な結果は、ソリューションが正常にビルド、単体テストに合格した場合、web のパッケージとその他の展開のリソース デプロイされているテスト環境にです。

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

**DeployToTest**定義装置 MSBuild にこれらの引数をビルドします。


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true**と**DeployTarget パッケージを =** プロパティは、ソリューション内のプロジェクト チーム ビルドによって作成されるときに使用されます。 Web アプリケーション プロジェクトをプロジェクトには、これらのプロパティは、プロジェクトの web 配置パッケージを作成するために MSBuild を指示します。 **TargetEnvPropsFile**プロパティは、通知、 *Publish.proj*ファイルをインポートする環境に固有のプロジェクト ファイルを検索します。

> [!NOTE]
> 次のようにビルド定義を作成する方法の詳細なチュートリアルについては、次を参照してください。[ビルド定義をサポートする展開を作成する](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)です。


*Publish.proj*ファイルには、ソリューション内の各プロジェクトをビルドするターゲットが含まれています。 ただし、ことをスキップし次のビルド ターゲットがチーム ビルドで、ファイルを実行している場合、条件付きロジックも含まれています。 これにより、チーム ビルドを提供する単体テストを実行する機能などの他のビルド機能を活用できます。 ソリューションのビルドまたは単体テストが失敗した場合、 *Publish.proj*ファイルは実行されないと、アプリケーションは展開されません。

条件付きロジックが評価することによって実現、 **BuildingInTeamBuild**プロパティです。 これは、MSBuild プロパティに自動的に設定されている**true**ときにビルドを使用してチーム プロジェクトをビルドします。

## <a name="deployment-to-staging"></a>ステージングへの展開

ビルドをテスト環境で開発チームの要件をすべて満たしている場合、チームが同じビルドをステージング環境に展開する必要可能性があります。 ステージング環境は、通常、実稼働または密接に限り、たとえば、サーバーの仕様、オペレーティング システムとソフトウェア、およびネットワークの構成の観点から「ライブ」の環境としての特性に一致するように構成します。 ステージング環境は、ロード テスト、ユーザー受け入れテスト、およびより広範な内部レビューのよく使用されます。 ビルドは、ビルド サーバーから直接ステージング環境に展開されます。

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

ステージング環境にソリューションを配置するために使用するビルド定義**DeployToStaging-whatif**と**DeployToStaging**、これらの特性を共有します。

- 何もを実際に作成がありません。 Rob でソリューションをステージング環境に展開すると、そこでは既に確認され、テスト環境で検証を特定すると、既存のビルドを配置することです。 だけ、ビルド定義は、展開プロセスを制御するカスタム プロジェクト ファイルを実行する必要があります。
- Rob には、ビルドがトリガーされたときにビルド パラメーターを彼は、ビルドには、ビルド サーバーから展開を作成し、リソースが含まれています。 を指定します。
- ビルド定義が自動的に実行されません。 Rob は、彼がステージング環境にソリューションを配置するときに、ビルドを手動でキューします。

これは、ステージング環境に配置するための高度なプロセスです。

1. ステージング環境の管理者、Rob Walters キューを使用して、ビルド、 **DeployToStaging-whatif**定義を作成します。 Rob では、ビルド定義のパラメーターを使用して、展開を考えているビルドを指定します。
2. **DeployToStaging-whatif**ビルド定義の実行"what-if"モードでカスタム プロジェクト ファイルです。 Rob は実際の展開を実行していたが、実際には一切変更されません、送信先の環境に、ログ ファイルが生成されます。
3. Rob では、ステージング環境で、展開の影響を確認するために、ログ ファイルを確認します。 具体的には、Rob チェックしたい追加された新機能は、新機能が更新されますおよび新機能が削除されます。
4. Rob がある場合、展開が既存のリソースまたはデータに望ましくない変更を加えるしないことを満たす、彼はキューに配置を使用して、ビルド、 **DeployToStaging**ビルド定義します。
5. **DeployToStaging**定義の実行に、カスタムのプロジェクト ファイルをビルドします。 これらの展開リソースに関するステージング環境に、プライマリ web サーバーに発行します。
6. Web Farm Framework (WFF) コント ローラーは、ステージング環境の web サーバーを同期します。 これで利用できるアプリケーション、サーバー ファーム内のすべての web サーバー。

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

**DeployToStaging**定義装置 MSBuild にこれらの引数をビルドします。


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile**プロパティは、通知、 *Publish.proj*ファイルをインポートする環境に固有のプロジェクト ファイルを検索します。 **OutputRoot**プロパティが組み込みの値をオーバーライドしを展開するリソースを含むビルド フォルダーの場所を示します。 Rob では、ビルドをキュー、ときに使用して、**パラメーター**  タブの更新後の値を提供する、 **OutputRoot**プロパティです。

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> 次のようにビルド定義を作成する方法の詳細については、次を参照してください。[特定のビルドを配置](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md)です。


**DeployToStaging-whatif**ビルド定義にはとして同じデプロイ ロジックが含まれています、 **DeployToStaging**定義を作成します。 ただし、追加の引数を含めて**WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


内で、 *Publish.proj*ファイル、 **WhatIf**プロパティは、"what-if"モードで展開のすべてのリソースを発行することを示します。 つまり、展開に出かけてしまってが展開先の環境で何も変更が実際には、ログ ファイルが生成されます。 これにより、提案された展開の影響を評価する&#x2014;で特定の新機能は、追加、取得とは、更新、および新機能が削除されてしまう&#x2014;実際に変更を加える前にします。

> [!NOTE]
> "What-if"の展開を構成する方法の詳細については、次を参照してください。 ["What-if"展開を実行する](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md)です。


ステージング環境に、プライマリ web サーバーにアプリケーションをデプロイした後、WFF は自動的に同期されているサーバー ファーム内のすべてのサーバー間、アプリケーションを使用します。

> [!NOTE]
> Web サーバーを同期するために WFF を構成する方法については、次を参照してください。 [Web Farm Framework でサーバー ファームを作成する](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md)です。


## <a name="deployment-to-production"></a>実稼働環境に展開

ステージング環境にビルドが承認されたときに、Fabrikam, Inc. チームは、運用環境にアプリケーションを発行できます。 実稼働環境では、アプリケーションが「ライブ」したりエンドユーザーの場合は、その対象ユーザーに達すると。

実稼働環境では、インターネットに接続する境界ネットワーク内です。 これは、ビルド サーバーを含む内部ネットワークから分離されます。 実稼働環境の管理者、Lisa Andrews は、ビルド サーバーから web 展開パッケージをコピーし、プライマリの実稼働 web サーバーで IIS にインポートする必要があります手動でします。

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

これは、実稼働環境に配置するための高度なプロセスです。

1. 開発チームには、ビルドは実稼働環境に展開の準備ができている Lisa が表示されます。 チームには、ビルド サーバー上の web 展開パッケージが格納フォルダー内の場所の Lisa が示されます。
2. Lisa は、ビルド サーバーから web パッケージを収集し、実稼働環境で、プライマリ web サーバーにコピーします。
3. Lisa では、IIS マネージャーを使用して、インポートして、プライマリ web サーバー上の web パッケージを公開します。
4. WFF コント ローラーは、実稼働環境で web サーバーを同期します。 これで利用できるアプリケーション、サーバー ファーム内のすべての web サーバー。

### <a name="how-does-the-deployment-process-work"></a>展開プロセスのしくみ

IIS マネージャーには、IIS の web サイトに web パッケージを発行するが容易インポート アプリケーション パッケージ ウィザードが含まれています。 この手順を実行する方法のチュートリアルについては、次を参照してください。 [Web パッケージを手動でインストールする](../web-deployment-in-the-enterprise/manually-installing-web-packages.md)です。

## <a name="conclusion"></a>まとめ

このトピックでは、一般的なエンタープライズ規模の web アプリケーションの展開のライフ サイクルの図は、用意されています。

このトピックでは、一連の web アプリケーションの配置のさまざまな側面に関するガイダンスを提供するチュートリアルの一部を形成します。 実際には、展開プロセスの各段階で追加のタスクと考慮事項の多くがあるし、に 1 つのチュートリアルのすべてに対応することはできません。 詳細については、これらのチュートリアルを参照してください。

- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 このチュートリアルでは、MSBuild および (Web 配置) IIS Web 配置ツールを使用して web 展開技術の包括的な概要を提供します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。 このチュートリアルでは、さまざまな配置シナリオをサポートするために Windows server の環境を構成する方法について説明します。
- [Web 配置を自動的に Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 このチュートリアルでは、デプロイ ロジックを TFS のビルド プロセスに統合する方法について説明します。
- [企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、組織が直面するより複雑な複雑な展開の一部を満たす方法のガイダンスを提供します。

> [!div class="step-by-step"]
> [前へ](enterprise-web-deployment-scenario-overview.md)
