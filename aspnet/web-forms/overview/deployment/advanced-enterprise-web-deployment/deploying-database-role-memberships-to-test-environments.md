---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: データベース ロールのメンバーシップをテスト環境に展開する |Microsoft Docs
author: jrjlee
description: このトピックでは、テスト環境にデプロイするソリューションの一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。 含むソリューションを展開するときにしています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a690d99df7a19c422fb217544ec183c311d1796f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828034"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>データベース ロールのメンバーシップをテスト環境に展開します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、テスト環境にデプロイするソリューションの一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。
> 
> ステージング環境または運用環境にデータベース プロジェクトを含むソリューションを展開するときに、データベース ロールにユーザー アカウントの追加を自動化する開発者通常必要ありません。 ほとんどの場合、開発者がどのデータベース ロールに追加する必要があるユーザー アカウントを知ることができず、これらの要件は、いつでも変更できます。 ただし、開発中にデータベース プロジェクトを含むソリューションをデプロイまたは環境をテストするときに、状況は別ではなく通常。
> 
> - 開発者通常再ソリューションをデプロイを定期的に、1 日に数回多くの場合。
> - 通常、データベースがデータベース ユーザーの作成し、すべてのデプロイ後のロールに追加する必要があります、つまりすべてのデプロイで再作成されます。
> - 通常、開発者には、ターゲット開発またはテスト環境を完全に制御があります。
> 
> このシナリオで自動的にデータベース ユーザーを作成し、展開プロセスの一部としてデータベース ロールのメンバーシップを割り当てると便利です。
> 
> 重要な要素は、この操作する必要がある条件付きの場合、ターゲット環境に基づくです。 操作をスキップする場合は、ステージングまたは運用環境にデプロイします。 操作を行うロールのメンバーシップを展開する場合は、開発者にデプロイする場合、またはテスト環境は、します。 このトピックでは、この課題に対処する 1 つの方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

このトピックを前提とします。

- 」の説明に従って、ソリューションのデプロイメントにプロジェクト ファイルの分割方法を使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。
- 」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。

データベース ユーザーを作成し、テスト環境にデータベース プロジェクトを展開するときに、ロールのメンバーシップを割り当てる、する必要があります。

- 必要なデータベースの変更を行うトランザクションの構造化照会言語 (TRANSACT-SQL) スクリプトを作成します。
- Sqlcmd.exe ユーティリティを使用して SQL スクリプトを実行する Microsoft Build Engine (MSBuild) ターゲットを作成します。
- テスト環境にソリューションを配置するときに、ターゲットを呼び出すように、プロジェクト ファイルを構成します。

このトピックでは、これらの手順を実行する方法を説明します。

## <a name="scripting-the-database-role-memberships"></a>データベース ロールのメンバーシップをスクリプト作成

Transact SQL スクリプトを作成するには、多くのさまざまな方法とでは、任意の場所を選択します。 最も簡単な方法では、Visual Studio 2010 で、ソリューション内でスクリプトを作成します。

**SQL スクリプトを作成するには**

1. **ソリューション エクスプ ローラー**ウィンドウで、データベース プロジェクト ノードを展開します。
2. 右クリックし、**スクリプト**フォルダーをポイントして**追加**、 をクリックし、**新しいフォルダー**します。
3. 型**テスト**としてフォルダーの名前と Enter キーを押します。
4. 右クリックし、**テスト**フォルダーをポイントして**追加**、 をクリックし、**スクリプト**します。
5. **新しい項目の追加** ダイアログ ボックスで、スクリプトに、わかりやすい名前を付け (たとえば、 **AddRoleMemberships.sql**)、順にクリックします**追加**。

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. *AddRoleMemberships.sql*ファイルに、TRANSACT-SQL ステートメントを追加します。

    1. SQL Server ログイン、データベースにアクセスするためのデータベース ユーザーを作成します。
    2. 任意の必要なデータベース ロールにデータベース ユーザーを追加します。
7. このファイルのようになります。

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. ファイルを保存します。

## <a name="executing-the-script-on-the-target-database"></a>対象のデータベースでスクリプトの実行

理想的には、データベース プロジェクトを展開するときに、配置後スクリプトの一部として必要な TRANSACT-SQL スクリプトを実行するとします。 ただし、配置後スクリプトでは、ソリューション構成またはビルド プロパティに基づく条件付きロジックを実行することはありません。 MSBuild プロジェクト ファイルから直接作成して、SQL スクリプトを実行する方法が、**ターゲット**sqlcmd.exe コマンドを実行する要素。 このコマンドを使用して、対象のデータベースでスクリプトを実行することができます。


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)します。


このコマンドを埋め込むには、MSBuild ターゲットで、前に、スクリプトを実行するどのような条件を考慮する必要があります。

- そのロールのメンバーシップを変更する前に、ターゲット データベースが存在する必要があります。 そのため、このスクリプトを実行する必要があります*後*データベース デプロイします。
- スクリプトは、テスト環境にのみ実行されるように条件を含める必要があります。
- "What if"展開を実行しているかどうかは&#x2014;つまり、配置スクリプトの生成が実際に実行しているかどうかは&#x2014;SQL スクリプトを実行することはできません。

説明されている分割プロジェクト ファイルの方法を使用しているかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、Contact Manager サンプルのソリューションで示したように、このような SQL スクリプトのビルド手順を分割することができます。

- 必須のアクセス許可をデプロイするかどうかを決定するプロパティと共に、環境固有のプロパティは、環境固有のプロジェクト ファイルに移動する必要があります (たとえば、 *Env Dev.proj*)。
- MSBuild ターゲット自体とその展開先の環境間変更されない任意のプロパティは、ユニバーサル プロジェクト ファイルに移動する必要があります (たとえば、 *Publish.proj*)。

環境固有のプロジェクト ファイルでは、データベース サーバー名、ターゲット データベース名、およびユーザー ロールのメンバーシップをデプロイするかどうかを指定するブール型プロパティを定義する必要があります。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


ユニバーサル プロジェクト ファイルでは、sqlcmd 実行可能ファイルの場所とを実行する SQL スクリプトの場所を指定する必要があります。 これらのプロパティは、移行先の環境に関係なく同じになります。 Sqlcmd コマンドを実行する MSBuild ターゲットを作成する必要があります。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


その他のターゲットに役立つことが考えられます sqlcmd 実行可能ファイルの場所は、静的なプロパティとして追加することに注意してください。 これに対し、として定義する SQL スクリプトの場所と sqlcmd コマンドの構文、ターゲット内の動的プロパティに属していない場合は必要なターゲットが実行される前にします。 ここで、 **DeployTestDBPermissions**ターゲットは、これらの条件が満たされた場合にのみ実行されます。

- **DeployTestDBRoleMemberships**プロパティに設定されて**true**します。
- ユーザーが指定されていない、 **WhatIf = true**フラグ。

最後に、忘れずに、ターゲットを起動します。 *Publish.proj*ファイル、これを行う既定の依存関係の一覧にターゲットを追加して**FullPublish**ターゲット。 いることを確認する必要がある、 **DeployTestDBPermissions**までターゲットは実行されません、 **PublishDbPackages**ターゲットが実行されています。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>まとめ

このトピックでは、1 つを追加する方法データベース ユーザーとロール メンバーシップ配置後アクションとしてデータベース プロジェクトをデプロイするときについて説明します。 これは、機能は、定期的にテスト環境でデータベースを再作成するが、通常はできるだけ避けてステージングまたは運用環境にデータベースを展開するときに通常は便利です。 そのため、データベース ユーザーおよびロールのメンバーシップが作成されるようのみこれを行うには適切なタイミングは、必要な条件付きロジックを使用することを確認してください。

## <a name="further-reading"></a>関連項目

VSDBCMD を使用してデータベース プロジェクトを配置する詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。 別のターゲット環境のデータベースの配置をカスタマイズする方法のガイダンスについては、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](customizing-database-deployments-for-multiple-environments.md)します。 カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。 Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)します。

> [!div class="step-by-step"]
> [前へ](customizing-database-deployments-for-multiple-environments.md)
> [次へ](deploying-membership-databases-to-enterprise-environments.md)
