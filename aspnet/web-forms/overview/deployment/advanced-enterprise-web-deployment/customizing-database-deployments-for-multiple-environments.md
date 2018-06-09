---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: 複数の環境のデータベースの配置をカスタマイズする |Microsoft ドキュメント
author: jrjlee
description: 'このトピックでは、展開プロセスの一部として特定のターゲット環境にデータベースのプロパティを調整する方法について説明します。 注: トピックには、番目が前提としています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 06f22bc9a3068ee5621df62ee5ed1bea06d7e9e6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30881358"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>複数の環境のデータベースの展開のカスタマイズ
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、展開プロセスの一部として特定のターゲット環境にデータベースのプロパティを調整する方法について説明します。
> 
> > [!NOTE]
> > トピックでは、MSBuild.exe と VSDBCMD.exe を使用して、Visual Studio 2010 データベース プロジェクトを配置することを前提としています。 この方法を選択する理由の詳細については、次を参照してください。 [、企業内の Web 配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)と[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。
> 
> 
> 複数の変換先にデータベース プロジェクトを展開するときを多くの場合を各ターゲット環境のデータベースの配置プロパティをカスタマイズします。 たとえば、テスト環境では通常を再作成するそれぞれの配置上のデータベース ステージングまたは運用環境で、データを保持するために増分更新を行うよりもずっと可能性の高いでしょうがします。
> 
> Visual Studio 2010 データベース プロジェクトでは、展開の設定が展開 (.sqldeployment) の構成ファイル内で格納されます。 このトピックでは、環境固有の配置構成ファイルを作成し、VSDBCMD パラメーターとして使用する 1 つを指定する方法を示します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

このトピックを前提とします。

- 」の説明に従って、ソリューションの配置をプロジェクト ファイルの分割アプローチを使用する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。
- 」の説明に従って、データベース プロジェクトを配置するプロジェクト ファイルから VSDBCMD を呼び出す[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。

さまざまなターゲット環境間でデータベースの配置プロパティをサポートする展開システムを作成するには、する必要があります。

- 各ターゲット環境の展開の構成 (.sqldeployment) ファイルを作成します。
- コマンド ライン スイッチと展開構成ファイルを指定する VSDBCMD コマンドを作成します。
- VSDBCMD オプションは、ターゲット環境に適しているために、Microsoft Build Engine (MSBuild) は、プロジェクト ファイルで VSDBCMD コマンドをパラメーター化します。

このトピックでは、各手順を実行する方法を示します。

## <a name="creating-environment-specific-deployment-configuration-files"></a>環境固有の配置構成ファイルを作成します。

既定では、データベース プロジェクトがという名前の単一の展開構成ファイルを含む*Database.sqldeployment*です。 このファイルを開くには、Visual Studio 2010 で、使用可能な各種展開オプションを確認できます。

- **配置の比較の照合順序**です。 これにより、プロジェクトのデータベースの照合順序を使用するかどうかを選択できます (、*ソース*照合順序) または、移行先サーバーのデータベースの照合順序 (、*ターゲット*照合順序)。 ほとんどの場合は、開発用に展開または、環境をテストするときに、ソースの照合順序を使用するします。 ステージングまたは運用環境に展開するときに、変更せずに相互運用性の問題を回避するターゲットの照合順序のままにしておく通常します。
- **データベースのプロパティを配置する**です。 定義されているデータベースのプロパティを適用するかどうかを選択できます、 *Database.sqlsettings*ファイル。 最初にデータベースを展開するときに、データベースのプロパティを展開する必要があります。 既存のデータベースを更新する場合は、プロパティの場所に既に存在する必要がありにもう一度配置する必要はありません。
- **常にデータベースを再作成**です。 これにより展開または増分変更するため、ターゲット データベースに最新の状態、スキーマを使用するたびに、ターゲット データベースを再作成するかどうかを選択できます。 データベースを再作成すると、既存のデータベース内のデータが失われます。 そのため、通常この設定の値**false**のステージング環境または実稼働環境に展開します。
- **データの損失が発生する場合に増分配置をブロック**です。 これにより、データベース スキーマの変更がデータの損失を引き起こす場合は配置を停止するかどうかを選択できます。 通常これを設定する**true**介入し、重要なデータを保護する機会を提供する、実稼働環境に配置するためです。 設定した場合**常にデータベースを再作成**に**false**、この設定には影響はありません。
- **シングル ユーザー モードで展開を実行する**です。 これは通常開発またはテスト環境で発生する問題ではありません。 ただしは通常これを設定する**true**のステージング環境または実稼働環境に展開します。 これはユーザーが、デプロイは進行中に、データベースに変更を加えることを防ぎます。
- **展開する前にデータベースをバックアップする**です。 通常これを設定する**true**データ損失に対する予防策として、実稼働環境への展開します。 設定することも**true**展開するときに、ステージング環境に、ステージング データベースには、大量のデータが含まれている場合。
- **ターゲット データベースにあってデータベース プロジェクトに含まれていないオブジェクトに対して DROP ステートメントを生成**です。 ほとんどの場合、これは、データベースの増分変更欠かせない重要な要素です。 設定した場合**常にデータベースを再作成**に**false**、この設定には影響はありません。
- **CLR 型の更新に ALTER ASSEMBLY ステートメントを使用しない**です。 この設定は、SQL Server が新しいアセンブリ バージョンに共通言語ランタイム (CLR) 型を更新する必要がある方法を決定します。 これに設定する必要があります**false**ほとんどのシナリオでします。

次の表は、別のインストール先の環境の典型的な展開設定を示します。 ただしの設定は、正確な要件に応じて異なる場合があります。

|  | 開発/テスト | ステージング/統合 | 実稼働 |
| --- | --- | --- | --- |
| **配置の比較の照合順序** | ソース | ターゲット | ターゲット |
| **データベースのプロパティを配置します。** | True | 最初の時刻のみ | 最初の時刻のみ |
| **常にデータベースを再作成します。** | True | False | False |
| **データの損失が発生する場合に増分配置をブロックします。** | False | おそらく | True |
| **シングル ユーザー モードで配置スクリプトを実行します。** | False | True | True |
| **展開する前にデータベースをバックアップします。** | False | おそらく | True |
| **ターゲット データベース内にあるオブジェクトに対して DROP ステートメントを生成しますが、データベース プロジェクトに含まれていません。** | False | True | True |
| **CLR 型の更新に ALTER ASSEMBLY ステートメントを使用しないでください。** | False | False | False |
  

> [!NOTE]
> データベースの配置プロパティと環境に関する考慮事項の詳細については、次を参照してください[、概要のデータベース プロジェクトの設定](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)、[する方法: 展開の詳細のプロパティを構成する](https://msdn.microsoft.com/library/dd172125.aspx)、 。[ビルドおよび分離開発環境にデータベースを配置](https://msdn.microsoft.com/library/dd193409.aspx)、および[ビルドし、ステージング環境または実稼働環境にデータベースを配置](https://msdn.microsoft.com/library/dd193413.aspx)です。


複数の送信先にデータベース プロジェクトの配置をサポートするには、各ターゲット環境の展開構成ファイルを作成する必要があります。

**環境に固有の構成ファイルを作成するには**

1. Visual Studio 2010 での**ソリューション エクスプ ローラー**  ウィンドウでは、データベース プロジェクトを右クリックし、をクリックして**プロパティ**です。
2. データベース プロジェクトのプロパティ ページで、**展開** タブの 、**展開構成ファイル**行で、をクリックして**新規**です。

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. **新しい配置構成ファイル** ダイアログ ボックスで、ファイルにわかりやすい名前を付けます (たとえば、 **TestEnvironment.sqldeployment**)、をクリックし、**保存**です。
4. *[Filename]***.sqldeployment** ページで、展開先の環境の要件に適合する配置プロパティを設定し、ファイルを保存します。

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. 新しいファイルが、データベース プロジェクトのプロパティ フォルダーに追加されたことに注意してください。

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>VSDBCMD の展開構成ファイルを指定します。

Visual Studio 2010 内 (デバッグやリリース) などのソリューション構成を使用する場合は、各構成で展開構成ファイルを関連付けることができます。 特定の構成をビルドすると、ビルド プロセスには、構成に固有の展開構成ファイルを指す構成固有の配置マニフェスト ファイルが生成されます。 ただし、これらのチュートリアルで説明する展開のアプローチの主な目的の 1 つは、Visual Studio 2010 とソリューション構成を使用せず、展開プロセスを制御する機能できるようにします。 この方法では、ソリューション構成は、ターゲット展開環境に関係なく同じです。 特定の送信先の環境に、データベースの配置を調整するため、展開構成ファイルを指定するのに、VSDBCMD コマンド ライン オプションを使用できます。

指定する展開構成ファイル、VSDBCMD を使用して、 **p:/DeploymentConfigurationFile**切り替えるし、ファイルへの完全パスを提供します。 これにより、配置マニフェストを識別する展開構成ファイルが上書きされます。 たとえば、展開するこの VSDBCMD コマンドを使用する可能性があります、 **ContactManager**テスト環境へのデータベース。


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> 出力ディレクトリにファイルをコピーするときに、ビルド プロセスに .sqldeployment ファイルを変更ことがありますに注意してください。


配置前や配置後 SQL スクリプトでの SQL コマンド変数を使用する場合に、展開で、特定の環境 .sqlcmdvars ファイルを関連付けるには、同様のアプローチを使用できます。 この場合、使用して、 **p:/SqlCommandVariablesFile** .sqlcmdvars ファイルを識別するスイッチです。

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>MSBuild プロジェクト ファイルから VSDBCMD コマンドを実行しています。

使用して MSBuild プロジェクト ファイルから VSDBCMD コマンドを呼び出すことができます、 **Exec** MSBuild ターゲット内のタスクです。 最も単純な形式には次のようになります。


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- 実際には、プロジェクト ファイルを簡単に読み取るし、再利用します、さまざまなコマンド ライン パラメーターを格納するプロパティを作成します。 これにより、環境固有のプロジェクト ファイル内のプロパティ値を提供するか、MSBuild コマンドラインからの既定値をオーバーライドにユーザーが簡単にします。 説明されている分割プロジェクト ファイルのアプローチを使用するかどうかは[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、それに応じて、ビルド方法および 2 つのファイルのプロパティを分割する必要があります。
- 展開構成ファイル名、データベース接続文字列で、ターゲット データベース名などの環境に固有の設定は、環境固有のプロジェクト ファイルに移動する必要があります。
- VSDBCMD の実行可能ファイルの場所のように汎用プロパティと共に、VSDBCMD コマンドを実行する MSBuild ターゲットは、ユニバーサル プロジェクト ファイルで移動する必要があります。

.Deploymanifest ファイルが作成され、使用するように VSDBCMD を呼び出す前に、データベース プロジェクトをビルドすることも確認する必要があります。 トピックでは、この方法の完全な例を確認できます[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)を順を追ってするでプロジェクト ファイル、[連絡先のマネージャーのサンプル ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)です。

## <a name="conclusion"></a>まとめ

このトピックでは、MSBuild および VSDBCMD を使用してデータベース プロジェクトを展開するときに、別のインストール先の環境にデータベースのプロパティを調整できる方法について説明します。 この方法は、エンタープライズ規模の大きい、ソリューションの一部としてデータベース プロジェクトを配置する必要がある場合に便利です。 これらのソリューションは、セキュリティで保護された開発環境またはテスト環境、ステージング環境または統合プラットフォームでは、および実稼働環境のライブなど、複数の送信先に多くの場合、展開されます。 通常の各ターゲット環境にデータベースの配置プロパティの一意のセットが必要です。

## <a name="further-reading"></a>関連項目

VSDBCMD.exe を使用してデータベース プロジェクトを展開する方法の詳細については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。 展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。

MSDN の次の記事では、データベースの配置より一般的なガイダンスを説明します。

- [データベース プロジェクトの設定の概要](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [方法: 展開の詳細のプロパティの構成](https://msdn.microsoft.com/library/dd172125.aspx)
- [ビルドおよび分離開発環境へのデータベースの配置](https://msdn.microsoft.com/library/dd193409.aspx)
- [ビルドし、ステージング環境または実稼働環境にデータベースを配置](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [前へ](performing-a-what-if-deployment.md)
> [次へ](deploying-database-role-memberships-to-test-environments.md)
