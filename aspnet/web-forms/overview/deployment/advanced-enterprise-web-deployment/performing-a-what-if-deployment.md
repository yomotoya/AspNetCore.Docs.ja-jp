---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 場合、何を実行する展開 |Microsoft Docs
author: jrjlee
description: このトピックの「' what-if ' を実行する方法について説明します (またはシミュレートされた) (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールと V を使用してデプロイいます.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f54a4d91ea0f735d3cd5b99c0dda5d83cbb5e5d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830650"
---
<a name="performing-a-what-if-deployment"></a>"What If"展開を実行します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックで"what-if"を実行する方法について説明します (またはシミュレートされた) (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツール、VSDBCMD を使用してデプロイします。 これにより、アプリケーションを実際に展開する前に、デプロイ ロジックの特定のターゲット環境に与える影響を判断できます。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルドおよび配置プロセスでは、2 つのプロジェクト ファイル&#x2014;いずれかすべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Web パッケージを"What If"展開を実行します。

Web Deploy には、"what-if"でのデプロイを実行する機能 (または試用版) が含まれます。 モード。 展開が実行される必要があるが、移行先サーバーでは何も変更が実際には、Web Deploy"what-if"モードでの成果物を配置するときにログ ファイルを生成します。 ログ ファイルを確認することは、デプロイは具体的には、移行先サーバーでがどのような影響を理解するのに役立ちます。

- どのような追加されます。
- 何が更新されます。
- 取得とは、削除します。

"What if"展開で実際に変更できません常に何が、移行先サーバーでは何もしないため、展開が成功するかどうかを予測します。

」の説明に従って[Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)、2 つの方法で Web Deploy を使用して web パッケージを展開する&#x2014;直接、またはを実行して MSDeploy.exe コマンド ライン ユーティリティを使用して、 *. deploy.cmd*ファイルビルド プロセスを生成します。

MSDeploy.exe を直接使用している場合は、追加することで、"what if"展開を行うことができます、 **– whatif**コマンドにフラグ。 たとえば、ContactManager.Mvc.zip パッケージをステージング環境にデプロイした場合、何が起こるかを評価するため、MSDeploy コマンドのようになりますこの。


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


削除することができます、"what if"展開の結果に満足したら、 **– whatif**ライブの展開を実行するフラグ。

> [!NOTE]
> MSDeploy.exe のコマンド ライン オプションの詳細については、次を参照してください。 [Web 配置操作の設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。


使用している場合、 *. deploy.cmd*ファイルを含めることで、"what if"展開を実行することができます、 **/t**フラグ (試用版モード) のフラグの代わりに、 **/y**のフラグ ("yes"、または更新モード)コマンド。 たとえばを実行して ContactManager.Mvc.zip パッケージを展開する場合に何が起こるかを評価するため、 *. deploy.cmd*ファイルでは、このコマンドのようになります。


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


「試用版モード」配置の結果に満足したら、置き換えることができます、 **/t**フラグを **/y**ライブの展開を実行するフラグ。


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> コマンド ライン オプションの詳細については *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。 実行する場合、 *. deploy.cmd*ファイル フラグのいずれかを指定せず、コマンド プロンプトが使用可能なフラグの一覧を表示します。


## <a name="performing-a-what-if-deployment-for-databases"></a>データベースの"What If"展開を実行します。

このセクションでは、VSDBCMD ユーティリティを使用して、増分"、"スキーマ ベースのデータベースの配置を実行していることを前提としています。 このアプローチがで詳しく説明されている[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。 理解するおくとこのトピックにここで説明した概念を適用する前にすることをお勧めします。

VSDBCMD を使用すると**デプロイ**モードを使用できます、 **/dd** (または **/DeployToDatabase**) VSDBCMD が実際にデータベースを配置またはだけを生成するかどうかを制御するフラグをデプロイ スクリプト。 .Dbschema ファイルを配置する場合、これは動作します。

- 指定した場合 **/dd+** または **/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。
- 指定した場合 **/dd-** またはスイッチは省略、VSDBCMD 配置スクリプトのみが生成されます。

> [!NOTE]
> .Dbschema ファイルの動作ではなく、.deploymanifest ファイルをデプロイする場合、 **/dd**スイッチはもっと複雑になります。 VSDBCMD は基本的には、値は無視、 **/dd** .deploymanifest ファイルが含まれている場合に切り替え、 **DeployToDatabase**の値を持つ要素**True**します。 [データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)を完全には、この動作について説明します。


たとえば、配置スクリプトを生成、 **ContactManager**この実際には、VSDBCMD コマンドは、データベースを展開することがなくデータベースのようになります。


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD は、データベースの差分の展開ツールと、指定したスキーマに存在する場合は、現在のデータベースを更新するために必要なすべての SQL コマンドを格納する配置スクリプトが動的に生成されるようします。 配置スクリプトを確認するは、配置に影響を与えるものを決定する便利な方法が、現在のデータベースとデータが含まれている必要があります。 たとえばを確認します。

- 任意の既存のテーブルを削除するかどうかと、データが失われるしまうかどうか。
- 操作の順序はたとえば、データ損失のリスクを実行するかどうか、分割またはテーブルをマージしている場合は。

配置スクリプトに満足したら場合に、VSDBCMD を繰り返すことができます、 **/dd+** を変更するフラグ。 または、要件を満たすし、データベース サーバーに手動で実行するには、配置スクリプトを編集できます。

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>カスタムのプロジェクト ファイルに"What If"機能を統合します。

複雑な展開シナリオでは、」の説明に従って、カスタム Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、ビルドと配置ロジックをカプセル化する必要あります[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。 たとえば、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションを*Publish.proj*ファイル。

- ソリューションをビルドします。
- パッケージ化し、ContactManager.Mvc アプリケーションをデプロイするには、Web Deploy を使用します。
- パッケージ化し、ContactManager.Service アプリケーションをデプロイするには、Web Deploy を使用します。
- 配置、 **ContactManager**データベース。

この方法でのシングル ステップのプロセスに複数の web のパッケージやデータベースの配置を統合するときは、"what-if"モードで展開全体を実行するオプションを必要もあります。

*Publish.proj*ファイルでは、これを実行する方法を示します。 最初に、"what-if"の値を格納するプロパティを作成する必要があります。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


この場合、という名前のプロパティを作成した**WhatIf**の既定値を持つ**false**します。 ユーザーは、プロパティを設定してこの値を上書きできます**true** 、コマンド ライン パラメーターでも後ほど説明します。

次に、任意の Web Deploy をパラメーター化して、VSDBCMD フラグを反映するためのコマンド、 **WhatIf**プロパティの値。 たとえば、次のターゲット (から取得した、 *Publish.proj*ファイルし、簡略化) を実行、 *. deploy.cmd* web パッケージを配置するファイル。 既定で、コマンドが含まれて、 **/Y**スイッチ ("yes"、または更新モード)。 場合**WhatIf**に設定されている**true**で置き換えられます、 **/T**スイッチ (試用版、または"what-if"モード)。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


同様に、次のターゲットは、VSDBCMD ユーティリティを使用して、データベースを配置します。 既定で、 **/dd**スイッチは含まれません。 つまり、VSDBCMD は配置スクリプトが生成されますが、データベースに展開できなくなります&#x2014;つまり、"what-if"シナリオ。 場合、 **WhatIf**プロパティに設定されていない**true**、 **/dd**スイッチが追加され、VSDBCMD はデータベースを配置します。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


同じアプローチを使用すると、プロジェクト ファイル内のすべての関連するコマンドをパラメーター化します。 "What if"展開を実行するときに行うことができますし、単に、 **WhatIf**コマンドラインからプロパティ値。


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


これにより、1 つのステップですべてのプロジェクト コンポーネント、"what if"展開を実行できます。

## <a name="conclusion"></a>まとめ

このトピックでは、Web Deploy、VSDBCMD、MSBuild を使用してデプロイ"what-if"を実行する方法について説明します。 "What if"展開では、移行先の環境に実際には、変更を加える前に、提案された展開の影響を評価することができます。

## <a name="further-reading"></a>関連項目

Web デプロイ コマンドライン構文の詳細については、次を参照してください。 [Web 配置操作の設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。 コマンド ライン オプションを使用する場合のガイダンスについては、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。 VSDBCMD コマンドライン構文については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンス。EXE (展開およびスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)します。

> [!div class="step-by-step"]
> [前へ](advanced-enterprise-web-deployment.md)
> [次へ](customizing-database-deployments-for-multiple-environments.md)
