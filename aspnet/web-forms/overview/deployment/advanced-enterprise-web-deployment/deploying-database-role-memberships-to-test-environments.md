---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "データベース ロール メンバーシップをテスト環境に展開する |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、テスト環境にソリューションの配置の一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。 含むソリューションを展開するときにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>データベース ロール メンバーシップをテスト環境に展開します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、テスト環境にソリューションの配置の一部として、データベース ロールにユーザー アカウントを追加する方法について説明します。
> 
> ステージングまたは運用環境にデータベース プロジェクトを含むソリューションを展開するときに通常しない開発者はデータベース ロールにユーザー アカウントの追加を自動化します。 ほとんどの場合、どのデータベース ロールに追加する必要があるユーザー アカウントを認識せず、開発者といつでもこれらの要件を変更する可能性があります。 ただし、開発用にデータベース プロジェクトを含むソリューションを配置または、環境をテストするときに、この状況は、通常ではなく別。
> 
> - 開発者は、通常再展開を定期的にソリューション 1 日に数回多くの場合。
> - 通常、データベースがすべての配置では、データベース ユーザーの作成し、すべての展開後に、ロールに追加する必要があることを意味で再作成されます。
> - 開発者は、ターゲット開発またはテスト環境を完全に制御を通常はします。
> 
> このシナリオでは、データベース ユーザーを自動的に作成し、展開プロセスの一部としてデータベース ロール メンバーシップを割り当てると役に立つは多くの場合です。
> 
> キー係数この操作する必要がある、条件付きターゲット環境に基づきます。 を、ステージングまたは運用環境に配置する場合は、操作をスキップします。 開発者に配置する環境をテストするか、展開するロールのメンバーシップをそれ以上の介入なし。 このトピックでは、この課題に対処する 1 つのアプローチについて説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

このトピックを前提とします。

- 」の説明に従って、ソリューションの配置をプロジェクト ファイルの分割アプローチを使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。
- 」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。

データベース ユーザーを作成し、データベース プロジェクトをテスト環境に配置するときに、ロールのメンバーシップを割り当てる、する必要があります。

- 必要なデータベースの変更は、Transact の構造化照会言語 (TRANSACT-SQL) スクリプトを作成します。
- SQL スクリプトを実行するには、sqlcmd.exe ユーティリティを使用して Microsoft Build Engine (MSBuild) ターゲットを作成します。
- テスト環境にソリューションを配置するときに、ターゲットを呼び出すように、プロジェクト ファイルを構成します。

このトピックでは、各手順を実行する方法を示します。

## <a name="scripting-the-database-role-memberships"></a>データベース ロールのメンバーシップをスクリプト作成

任意の場所を選択して、多くのさまざまな方法は、TRANSACT-SQL スクリプトを作成できます。 最も簡単な方法では、Visual Studio 2010 で、ソリューション内でスクリプトを作成します。

**SQL スクリプトを作成するには**

1. **ソリューション エクスプ ローラー**ウィンドウで、データベース プロジェクト ノードを展開します。
2. 右クリックし、**スクリプト**フォルダーを指す**追加**、順にクリック**新しいフォルダー**です。
3. 型**テスト**としてフォルダーの名前とし、Enter キーを押します。
4. 右クリックし、**テスト**フォルダーを指す**追加**、順にクリック**スクリプト**です。
5. **新しい項目の追加** ダイアログ ボックスで、スクリプトにわかりやすい名前を付けます (たとえば、 **AddRoleMemberships.sql**)、をクリックし、**追加**です。

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. *AddRoleMemberships.sql*ファイル、TRANSACT-SQL ステートメントを追加します。

    1. データベースにアクセスする SQL Server ログインのデータベース ユーザーを作成します。
    2. データベース ユーザーを必要なデータベース ロールに追加します。
7. ファイルは、このようになります。

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. ファイルを保存します。

## <a name="executing-the-script-on-the-target-database"></a>対象のデータベースでのスクリプトを実行します。

理想的には、データベース プロジェクトを展開するときに、配置後スクリプトの一部として、必要な TRANSACT-SQL スクリプトを実行するとします。 ただし、配置後スクリプトは、ソリューションの構成またはビルド プロパティに基づく条件付きロジックを実行することはできません。 代替手段が MSBuild プロジェクト ファイルから直接作成することで、SQL スクリプトを実行するには、**ターゲット**sqlcmd.exe コマンドを実行する要素。 このコマンドを使用すると、ターゲット データベースで、スクリプトを実行します。


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)です。


MSBuild ターゲットでこのコマンドを埋め込むと、前に、スクリプトを実行するどのような条件を考慮する必要があります。

- そのロールのメンバーシップを変更する前に、ターゲット データベースが存在する必要があります。 そのため、このスクリプトを実行する必要があります*後*データベース デプロイします。
- スクリプトは、テスト環境でのみ実行されるように条件を含める必要があります。
- "What-if"展開 & #x 2014; を実行している場合つまり、配置スクリプトを生成するが、実際にその & #x 2014; を実行している場合するべきではありません SQL スクリプトを実行します。

説明されている分割プロジェクト ファイルのアプローチを使用しているかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)連絡先のマネージャーのサンプル ソリューションで示したように、次のように、SQL スクリプトのビルド手順を分割することができます。

- 必要なアクセス許可を展開するかどうかを決定するプロパティと共に、環境に固有のプロパティが環境に固有のプロジェクト ファイルに移動する必要があります (たとえば、 *Env Dev.proj*)。
- その展開先環境間で変化しない任意のプロパティ自体には、MSBuild ターゲット ユニバーサル プロジェクト ファイル内で移動する必要があります (たとえば、 *Publish.proj*)。

環境に固有のプロジェクト ファイルでは、データベース サーバー名、ターゲット データベース名およびロールのメンバーシップを展開するかどうかを指定するブール型プロパティを定義する必要があります。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


ユニバーサル プロジェクト ファイルでは、sqlcmd の実行可能ファイルの場所とを実行する SQL スクリプトの場所を指定する必要があります。 これらのプロパティは、展開先の環境に関係なく同じになります。 また、sqlcmd コマンドを実行する MSBuild ターゲットを作成する必要があります。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


他のターゲットに役立つ可能性があります、sqlcmd の実行可能ファイルの場所は、静的なプロパティとして追加することを確認します。 これに対し、定義する、SQL スクリプトの場所と sqlcmd コマンドの構文、ターゲット内の動的なプロパティとして可能性が必要なターゲットが実行される前に、します。 ここで、 **DeployTestDBPermissions**ターゲットは、これらの条件が満たされた場合にのみ実行されます。

- **DeployTestDBRoleMemberships**プロパティに設定されている**true**です。
- ユーザーが指定されていない、 **WhatIf = true**フラグ。

最後に、忘れずにターゲットを起動します。 *Publish.proj*ファイル、これを行うターゲットを既定の依存関係の一覧に追加することによって**FullPublish**ターゲットです。 いることを確認する必要があります、 **DeployTestDBPermissions**までターゲットは実行されません、 **PublishDbPackages**ターゲットが実行されました。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>まとめ

このトピックでは、1 つを追加する方法データベース ユーザーとロールのメンバーシップ配置後アクションとして、データベース プロジェクトを展開するときに説明されています。 これは、機能は、定期的にテスト環境でデータベースを再作成するが、通常できるだけ避けてステージングまたは運用環境にデータベースを展開するときに一般的に便利です。 そのため、データベース ユーザーとロールのメンバーシップが作成されるようにのみこれを行うことをお勧めときに、必要な条件付きロジックを使用することを確認する必要があります。

## <a name="further-reading"></a>関連項目

VSDBCMD を使用してデータベース プロジェクトを配置する方法の詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。 別のターゲット環境のデータベースの配置をカスタマイズする方法のガイダンスについては、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](customizing-database-deployments-for-multiple-environments.md)です。 展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。 Sqlcmd コマンド ライン オプションの詳細については、次を参照してください。 [sqlcmd ユーティリティ](https://msdn.microsoft.com/library/ms162773.aspx)です。

>[!div class="step-by-step"]
[前へ](customizing-database-deployments-for-multiple-environments.md)
[次へ](deploying-membership-databases-to-enterprise-environments.md)
