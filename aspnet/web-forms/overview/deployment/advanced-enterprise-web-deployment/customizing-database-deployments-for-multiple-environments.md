---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: 複数の環境のデータベースの配置のカスタマイズ |Microsoft Docs
author: jrjlee
description: 'このトピックでは、展開プロセスの一部として特定のターゲット環境にデータベースのプロパティを調整する方法について説明します。 注: トピック th 前提としています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 3a368e5055f4921b68f5c5eb2739728a2f1fd4d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378074"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>複数の環境のデータベースの配置をカスタマイズします。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、展開プロセスの一部として特定のターゲット環境にデータベースのプロパティを調整する方法について説明します。
> 
> > [!NOTE]
> > トピックでは、MSBuild.exe と VSDBCMD.exe を使用して Visual Studio 2010 データベース プロジェクトをデプロイすることを前提としています。 この方法を選択する理由の詳細については、次を参照してください。 [、企業の Web 展開](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)と[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。
> 
> 
> 複数の変換先にデータベース プロジェクトを展開するときは、各ターゲット環境のデータベースの配置プロパティをカスタマイズするとする多くの場合。 たとえば、テスト環境では通常再作成するデプロイごとに、データベース ステージングまたは運用環境では多く可能性が高く、データを保持するために増分更新を行うには。
> 
> Visual Studio 2010 データベース プロジェクトでは、展開の設定は、展開構成 (.sqldeployment) ファイル内に格納されます。 このトピックでは、環境に固有の展開構成ファイルを作成し、VSDBCMD パラメーターとして使用する 1 つを指定する方法を説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

このトピックを前提とします。

- 」の説明に従って、ソリューションのデプロイメントにプロジェクト ファイルの分割方法を使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。
- 」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。

ターゲット環境間でデータベースの配置プロパティの変更をサポートする展開システムを作成するには、する必要があります。

- 各ターゲット環境の展開構成 (.sqldeployment) ファイルを作成します。
- コマンド ライン スイッチと展開構成ファイルを指定する VSDBCMD コマンドを作成します。
- Microsoft Build Engine (MSBuild) プロジェクト ファイルでは、VSDBCMD コマンドは、VSDBCMD オプションが ターゲット環境に適切なパラメーター化します。

このトピックでは、これらの手順を実行する方法を説明します。

## <a name="creating-environment-specific-deployment-configuration-files"></a>環境に固有の展開構成ファイルを作成します。

既定では、データベース プロジェクトがという名前の単一の展開構成ファイルを含む*Database.sqldeployment*します。 Visual Studio 2010 でこのファイルを開く場合に使用できるさまざまな展開オプションを確認できます。

- **デプロイの比較の照合順序**します。 これにより、プロジェクトのデータベースの照合順序を使用するかどうかを選択できます (、*ソース*照合順序) または移行先サーバーのデータベースの照合順序 (、*ターゲット*照合順序)。 ほとんどの場合は、ソースの照合順序を使用して、開発用に展開または環境をテストするときにします。 ステージング環境または運用環境にデプロイするときは、相互運用性の問題を回避するためにそのままターゲットの照合順序のままにするとする、通常は。
- **データベースのプロパティを配置する**します。 定義されているデータベースのプロパティを適用するかどうかを選択できます、 *Database.sqlsettings*ファイル。 最初にデータベースを展開するときに、データベースのプロパティを展開する必要があります。 既存のデータベースを更新する場合は、プロパティは、既に配置、されている必要があり、し、再度展開する必要はありません。
- **常にデータベースを再作成**です。 これにより再デプロイまたは増分変更する、ターゲット データベースに最新のスキーマを使用するたびにターゲット データベースを作成するかどうかを選択できます。 データベースを再作成すると、既存のデータベース内のデータが失われます。 そのため、する必要があります、通常、これを設定して**false**のステージング環境または運用環境に展開します。
- **データの損失が発生する場合は、増分配置をブロック**します。 これにより、データベース スキーマの変更がデータの損失を引き起こす場合は配置を停止するかどうかを選択できます。 設定する通常**true**介入して、重要なデータを保護することができます、運用環境にデプロイします。 設定した場合**常にデータベースを再作成**に**false**、この設定は効果がありません。
- **シングル ユーザー モードで展開を実行する**します。 これは通常、開発またはテスト環境で、問題ではありません。 ただしは通常、これを設定して**true**のステージング環境または運用環境に展開します。 これはユーザーがデプロイが進行中に、データベースに変更を加えることを防ぎます。
- **配置前にデータベースをバックアップ**します。 設定する通常**true**データ損失の防止のため、実稼働環境へのデプロイします。 設定することも**true**展開する場合に、ステージング環境、ステージング データベースには、大量のデータが含まれている場合。
- **ターゲット データベースにあってデータベース プロジェクトにないオブジェクトに対して DROP ステートメントを生成**します。 ほとんどの場合、これは、データベースの増分変更欠かせない重要な要素です。 設定した場合**常にデータベースを再作成**に**false**、この設定は効果がありません。
- **CLR 型の更新に ALTER ASSEMBLY ステートメントを使用しない**します。 この設定は、SQL Server が新しいアセンブリ バージョンに共通言語ランタイム (CLR) 型を更新する必要がある方法を決定します。 これに設定する必要があります**false**ほとんどのシナリオでします。

次の表では、別の変換先の環境の一般的な展開設定を示します。 ただし、設定は、正確な要件に応じて異なる可能性があります。

|  | 開発/テスト | ステージング/統合 | 実稼働 |
| --- | --- | --- | --- |
| **デプロイの比較の照合順序** | ソース | ターゲット | ターゲット |
| **データベースのプロパティを配置します。** | True | 最初の時刻のみ | 最初の時刻のみ |
| **データベースを常に再作成** | True | False | False |
| **データの損失が発生する場合は、増分配置をブロックします。** | False | おそらく | True |
| **シングル ユーザー モードで配置スクリプトを実行します。** | False | True | True |
| **展開する前にデータベースをバックアップします。** | False | おそらく | True |
| **ターゲット データベースにあるオブジェクトに対して DROP ステートメントを生成しますが、データベース プロジェクトではないです。** | False | True | True |
| **CLR 型の更新に ALTER ASSEMBLY ステートメントを使用しません。** | False | False | False |
  

> [!NOTE]
> データベースの配置プロパティと環境の考慮事項の詳細については、次を参照してください[、概要のデータベース プロジェクトの設定](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)、[方法: プロパティの展開の詳細構成](https://msdn.microsoft.com/library/dd172125.aspx)、 。[ビルドおよび分離開発環境にデータベースを配置](https://msdn.microsoft.com/library/dd193409.aspx)、および[をビルドおよびステージング環境または運用環境にデータベースを配置](https://msdn.microsoft.com/library/dd193413.aspx)します。


複数の送信先へのデータベース プロジェクトの配置をサポートするには、各ターゲット環境の展開構成ファイルを作成する必要があります。

**環境固有の構成ファイルを作成するには**

1. Visual Studio 2010 での**ソリューション エクスプ ローラー**ウィンドウでは、データベース プロジェクトを右クリックし、順にクリックします**プロパティ**します。
2. データベース プロジェクトのプロパティ ページで、**デプロイ** タブで、**展開構成ファイル**行で、をクリックして**新規**します。

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. **新しい展開構成ファイル** ダイアログ ボックスで、ファイルのわかりやすい名前を付けます (たとえば、 **TestEnvironment.sqldeployment**)、順にクリックします**保存**。
4. *[Filename]***.sqldeployment** ページで、展開先の環境の要件に適合する配置プロパティを設定し、ファイルを保存します。

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. データベース プロジェクトで、[プロパティ] フォルダーに新しいファイルが追加されたことに注意してください。

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>VSDBCMD で展開構成ファイルを指定します。

Visual Studio 2010 内で (デバッグやリリース) などのソリューション構成を使用するときに構成ごとに、展開構成ファイルを関連付けることができます。 特定の構成をビルドすると、ビルド プロセスには、構成に固有の展開の構成ファイルを指す構成固有の配置マニフェスト ファイルが生成されます。 ただし、これらのチュートリアルで説明する展開のアプローチの主な目的の 1 つは、Visual Studio 2010 とソリューション構成を使用せず、展開プロセスを制御する機能をユーザーに付与します。 この方法では、ソリューションの構成は、ターゲット展開環境に関係なく同じです。 特定の送信先の環境に、データベースの配置を調整するには、展開構成ファイルを指定するのに、VSDBCMD コマンド ライン オプションを使用できます。

展開構成ファイルを VSDBCMD を指定するには使用、 **/p: DeploymentConfigurationFile**切り替えるし、ファイルへの完全パスを指定します。 これにより、配置マニフェストを識別する展開構成ファイルが上書きされます。 たとえば、展開するこの VSDBCMD コマンドを使用する可能性があります、 **ContactManager**テスト環境にデータベース。


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> 出力ディレクトリにファイルをコピーするときに、ビルド プロセスが .sqldeployment ファイルを変更可能性がありますに注意してください。


配置前または配置後の SQL スクリプトでの SQL コマンド変数を使用する場合、デプロイで、環境固有の .sqlcmdvars ファイルを関連付けるには、同様のアプローチを使用できます。 この場合、使用して、 **/p: SqlCommandVariablesFile** .sqlcmdvars ファイルを識別するためにスイッチします。

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>MSBuild プロジェクト ファイルから、VSDBCMD コマンドを実行しています。

使用して MSBuild プロジェクト ファイルから VSDBCMD コマンドを呼び出すことができます、 **Exec** MSBuild ターゲット内のタスク。 最も単純な形式でこのようなことになります。


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- 実際には、プロジェクト ファイルを簡単に読み取るし、再利用するために、さまざまなコマンド ライン パラメーターを格納するプロパティを作成します。 これにより、環境固有のプロジェクト ファイルのプロパティの値を提供する、または MSBuild のコマンドラインからの既定値をオーバーライドするユーザーを簡単にします。 説明されている分割プロジェクト ファイルの方法を使用するかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、それに応じて、ビルド方法および 2 つのファイルのプロパティを分割する必要があります。
- 展開構成ファイル名、データベース接続文字列および先のデータベース名などの環境に固有の設定は、環境固有のプロジェクト ファイルに移動してください。
- VSDBCMD の実行可能ファイルの場所のような汎用プロパティと共に、VSDBCMD コマンドを実行する MSBuild ターゲットは、ユニバーサル プロジェクト ファイルに移動してください。

また、.deploymanifest ファイルが作成され使用できるように、VSDBCMD を起動する前に、データベース プロジェクトをビルドすることを確認する必要があります。 トピックでは、この方法の完全な例を確認できます[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、プロジェクトのファイルで説明しています、 [Contact Manager サンプル ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)します。

## <a name="conclusion"></a>まとめ

このトピックでは、MSBuild と VSDBCMD を使用してデータベース プロジェクトを配置するときに別の変換先の環境にデータベースのプロパティを調整について説明します。 このアプローチは、エンタープライズ規模の拡大、ソリューションの一部としてデータベース プロジェクトを配置する必要がある場合に便利です。 これらのソリューションは、サンド ボックス化された開発またはテスト環境、ステージング環境または統合プラットフォームでは、運用環境または実稼働環境など、複数の送信先に多くの場合、デプロイされます。 通常の各ターゲット環境にデータベースの配置プロパティの一意のセットが必要です。

## <a name="further-reading"></a>関連項目

VSDBCMD.exe を使用してデータベース プロジェクトの配置の詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)します。 カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。

MSDN でこれらの記事では、データベースの配置に関する一般的なガイダンスを提供します。

- [データベース プロジェクトの設定の概要](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [方法: 展開の詳細のプロパティの構成](https://msdn.microsoft.com/library/dd172125.aspx)
- [ビルドおよび分離開発環境にデータベースを配置](https://msdn.microsoft.com/library/dd193409.aspx)
- [ビルドおよびステージング環境または運用環境にデータベースを配置](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [前へ](performing-a-what-if-deployment.md)
> [次へ](deploying-database-role-memberships-to-test-environments.md)
