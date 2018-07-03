---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: データベース プロジェクトの配置 |Microsoft Docs
author: jrjlee
description: '注: エンタープライズ展開シナリオの多くは、する必要が配置されたデータベースに増分更新を発行する機能。 代わりでは、再作成しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 3d261acbd6f8dab60a02c21546d8bc276de486ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374964"
---
<a name="deploying-database-projects"></a>データベース プロジェクトの配置
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> エンタープライズ展開シナリオの多くは、配置されたデータベースに増分更新を発行する機能が必要です。 代わりに、すべてのデプロイでは、既存のデータベース内のデータが失われることを意味上のデータベースを再作成することです。 Visual Studio 2010 を使用する場合は、増分データベース発行に推奨される方法は VSDBCMD を使用します。 ただし、次のバージョンの Visual Studio と Web 発行パイプライン (WPP) 増分直接公開をサポートするツールが含まれます。


Contact Manager サンプル ソリューションを開くには、Visual Studio 2010 で、データベース プロジェクトに 4 つのファイルを含むプロパティ フォルダーが含まれることがわかります。

![](deploying-database-projects/_static/image1.png)

プロジェクト ファイルと共に (*ContactManager.Database.dbproj*ここでは)、これらのファイルがビルドおよび配置プロセスのさまざまな側面を制御します。

- *Database.sqlcmdvars*ファイルがプロジェクトを配置するときに使用するすべての SQLCMD 変数の値を提供します。 各ソリューション構成 (たとえば、デバッグとリリース) には、さまざまな .sqlcmdvars ファイルを指定できます。
- *Database.sqldeployment*ファイル、プロジェクトまたは移行先サーバーの照合順序で定義されている照合順序を使用するかどうかのように、展開に固有の設定は、転送先データベースを再作成するかどうかごと時間または単純にして、最新では、既存のデータベースを修正します。 各ソリューション構成では、さまざまな .sqldeployment ファイルを指定できます。
- *Database.sqlpermissions*ファイルは、ターゲット データベースに追加するすべての権限の定義に使用できる XML ドキュメント。 すべてのソリューション構成では、同じ .sqlpermissions ファイルを共有します。
- *Database.sqlsettings*ファイルが、比較演算子の動作を使用する照合順序など、データベースを作成するときに使用するデータベース レベルのプロパティを指定します。 すべてのソリューション構成では、同じ .sqlsettings ファイルを共有します。

ですが、Visual Studio でこれらのファイルを開き、内容を理解しておきます。

データベース プロジェクトをビルドすると、ビルド プロセスには、2 つのファイルが作成されます。

- A*データベース スキーマ*(.dbschema ファイル)。 これには、XML 形式で作成するデータベースのスキーマについて説明します。
- A*配置マニフェスト*(.deploymanifest ファイル)。 これには、作成して、データベースをデプロイするために必要なすべての情報が含まれています。 デプロイの手順 (.sqldeployment ファイル) と、配置前または配置後の SQL スクリプトのように、他のリソースと一緒に .dbschema ファイルを参照します。

これは、これらのリソース間の関係を示します。

![](deploying-database-projects/_static/image2.png)

ご覧のとおり、.sqlsettings ファイルと .sqlpermissions ファイルには、ビルド プロセスに入力です。 データベース プロジェクト ファイルで、と共に、これらのファイルは、データベースのスキーマ ファイルを作成に使用されます。 .Sqldeployment ファイルと .sqlcmdvars ファイルは、変更、ビルド プロセスを通過します。 配置マニフェストでは、データベース スキーマ、.sqldeployment ファイル、.sqlcmdvars ファイル、および配置前または配置後の SQL スクリプトの場所を示します。

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>VSDBCMD を使用して、データベース プロジェクトを配置する理由ですか。

データベース プロジェクトを展開する各種のさまざまな方法はあります。 ただし、それらのすべては、エンタープライズ環境でのリモート サーバーにデータベース プロジェクトを展開するのに適しています。 データベース プロジェクトの配置からたいことを検討してください。 エンタープライズ展開シナリオでは、するできました。

- リモートの場所からデータベース プロジェクトを配置する権限です。
- 既存のデータベースを増分更新する権限です。
- 配置前スクリプトまたは配置後スクリプトを含むことのできます。
- 複数の展開先環境へのデプロイを調整する機能。
- 大きくの一部としてデータベース プロジェクトをデプロイできる通常、スクリプト化された 1 つの手順から成るソリューションの配置。

データベース プロジェクトを展開に使用できる 3 つの主な方法はあります。

- Visual Studio 2010 データベース プロジェクトの種類でデプロイ機能を使用できます。 ビルドして、Visual Studio 2010 データベース プロジェクトを展開するとき、展開プロセスは、ビルド構成に固有の SQL ベースの展開ファイルを生成するのに、配置マニフェストを使用します。 既に存在かが既に存在する場合は、データベースに必要な変更を加える場合は、データベースが作成されます。 SQLCMD.exe を使用するには、移行先サーバーでこのファイルを実行するか作成し、ファイルを実行する Visual Studio を設定することができます。 このアプローチの欠点は、展開の設定の制御がのみに制限されています。 多くの場合も、特定の環境変数の値を指定する SQL デプロイ ファイルを変更する必要があります。 Visual Studio 2010 がインストールされている場合、このアプローチのコンピューターからのみ使用でき、開発者が知るし、すべての展開先環境の接続文字列と資格情報を提供する必要があります。
- インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用する[web アプリケーション プロジェクトの一部としてデータベースを展開](https://msdn.microsoft.com/library/dd465343.aspx)します。 ただし、この手法は、移行先サーバー上の既存のローカル データベースをレプリケートするだけではなく、データベース プロジェクトを配置する場合は、もっと複雑です。 Web 配置のためにが、データベース プロジェクトを生成する SQL デプロイ スクリプトを実行することができます、web アプリケーション プロジェクトのカスタム WPP ターゲット ファイルを作成する必要があります。 展開プロセスに膨大な量の複雑さが追加されます。 さらに、Web Deploy 直接サポートしませんの増分更新を既存のデータベース。 この方法の詳細については、次を参照してください。 [SQL ファイルを展開パッケージのデータベース プロジェクトに Web 発行パイプラインを拡張する](https://go.microsoft.com/?linkid=9805121)します。
- VSDBCMD ユーティリティを使用して、データベース スキーマまたは配置マニフェストのいずれかを使用して、データベースをデプロイすることができます。 VSDBCMD.exe は、MSBuild ターゲット データベースをスクリプト化されたより大規模の展開プロセスの一環として発行できますから呼び出すことができます。 .Sqlcmdvars ファイルとその他のデータベースのプロパティを複数のビルド構成を作成せず、展開のさまざまな環境をカスタマイズすることができます、VSDBCMD のコマンドから多数の変数をオーバーライドできます。 VSDBCMD は、データベース スキーマを転送先データベースを配置するために必要な変更のみが行われますが、差別化機能を提供します。 VSDBCMD は、さまざまなコマンド ライン オプションは、展開プロセスをよりきめ細かく制御も提供します。

この概要については、MSBuild で VSDBCMD を使用するが一般的な企業の展開シナリオに最適な方法がわかります。

|  | Visual Studio 2010 | Web 配置 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| リモート展開をサポートしますか。 | [はい] | はい | [はい] |
| 増分更新をサポートしていますか。 | [はい] | いいえ | [はい] |
| 前/配置スクリプトをサポートしていますか。 | [はい] | はい | [はい] |
| マルチ環境の展開をサポートしていますか。 | 制限 | 制限 | [はい] |
| スクリプト化されたデプロイをサポートしていますか。 | 制限 | [はい] | [はい] |

このトピックの残りの部分では、VSDBCMD をデータベース プロジェクトを配置するために MSBuild の使用について説明します。

## <a name="understanding-the-deployment-process"></a>展開プロセスを理解します。

VSDBCMD ユーティリティを使用して、データベース スキーマ (.dbschema ファイル) または配置マニフェスト (.deploymanifest ファイル) を使用してデータベースを展開できます。 実際には、配置マニフェストを使用すると、さまざまな展開のプロパティの既定値を指定し、配置前または配置後 SQL スクリプトを実行するように、配置マニフェストをほぼ常に使用します。 デプロイにこの VSDBCMD コマンドを使用するなど、 **ContactManager**データベースをテスト環境でデータベース サーバー。


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


この場合、次のようになります。

- **/A** (または **/Action**) スイッチでは、VSDBCMD を実行するを指定します。 これを設定して**インポート**または**デプロイ**します。 **インポート**オプションを使用して、既存のデータベースから .dbschema ファイルを生成し、**デプロイ**.dbschema ファイルをターゲット データベースに展開するオプションを使用します。
- **/Manifest** (または **/ManifestFile**) スイッチを展開する .deploymanifest ファイルを識別します。 代わりに .dbschema ファイルを使用したい場合は使用、**モデル/** (または **/ModelFile**) スイッチします。
- **/Cs** (または **/ConnectionString**) スイッチでは、対象のデータベース サーバーの接続文字列を提供します。 データベースの名前が含まれていない、この注&#x2014;VSDBCMD は、データベースを作成するサーバーに接続する必要があります個々 のデータベースに接続する必要はありません。 .Deploymanifest ファイルには、接続文字列が含まれている場合は、このスイッチを省略できます。 とにかく、スイッチを使用する場合、スイッチの値は .deploymanifest 値をオーバーライドします。
- <strong>/P:TargetDatabase</strong>プロパティをターゲット データベースの作成時に割り当てる名前を提供します。 値が上書きされます。、 <strong>TargetDatabase</strong> .deploymanifest ファイルのプロパティ。 使用することができます、 <strong>/p:</strong> <em>[プロパティ名]</em>.sqlcmdvars ファイルで宣言されている構文をさまざまな展開のプロパティを設定して、すべての SQLCMD 変数をオーバーライドします。
- **/Dd+** (または **/DeployToDatabase+**) スイッチでは、展開を作成し、ターゲット環境にデプロイすることを示します。 指定した場合 **/dd-** スイッチは省略または、VSDBCMD は配置スクリプトが生成されますが、ターゲット環境に配置されません。 このスイッチは、混乱の原因は、多くの場合とは、次のセクションで詳しく説明します。
- **/Script** (または **/DeploymentScriptFile**) スイッチでは、配置スクリプトを生成するを指定します。 この値は、展開プロセスには影響しません。

VSDBCMD の詳細については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンス。EXE (展開およびスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)と[方法: VSDBCMD を使用して、コマンド プロンプトからの展開データベースを準備します。EXE](https://msdn.microsoft.com/library/dd193258.aspx)します。

MSBuild プロジェクト ファイルから、VSDBCMD を使用する方法の例は、次を参照してください。[ビルド プロセスを理解する](understanding-the-build-process.md)します。 複数の環境のデータベースの配置設定を構成する方法の例については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)します。

## <a name="understanding-the-deploytodatabase-switch"></a>DeployToDatabase スイッチを理解します。

動作、 **/dd**または **/DeployToDatabase**スイッチは、VSDBCMD .dbschema ファイルまたは .deploymanifest ファイルを使用するかどうかによって異なります。 .Dbschema ファイルを使用している場合の動作は非常に簡単です。

- 指定した場合 **/dd+** または **/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。
- 指定した場合 **/dd-** またはスイッチは省略、VSDBCMD 配置スクリプトのみが生成されます。

.Deploymanifest ファイルを使用している場合の動作ははるかに複雑です。 これは、.deploymanifest ファイルには、プロパティ名が含まれているため**DeployToDatabase**も、データベースが展開されているかどうかを決定します。


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


このプロパティの値は、データベース プロジェクトのプロパティに従って設定されます。 設定した場合、**アクション展開**に**デプロイ スクリプト (.sql) 作成**、値になります**False**。 設定した場合、**アクション展開**に**デプロイ スクリプト (.sql) を作成し、データベースに展開**、値になります**True**します。

> [!NOTE]
> これらの設定は、特定のビルド構成とプラットフォームに関連付けられます。 設定を構成する場合など、**デバッグ**構成し、発行を使用して、**リリース**の構成の設定は使用されません。


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> このシナリオで、**アクション展開**に常に設定する必要があります**デプロイ スクリプト (.sql) 作成**Visual Studio 2010、データベースの配置をしたくないので。 つまり、 **DeployToDatabase**プロパティは常に**False**します。


ときに、 **DeployToDatabase**プロパティが指定されて、 **/dd**プロパティの値がある場合は、スイッチに、プロパティはオーバーライドのみ**false**:

- 場合、 **DeployToDatabase**プロパティは**False**を指定して **/dd+** または **/dd**、VSDBCMD を上書、 **DeployToDatabase**プロパティおよびデータベースを展開します。
- 場合、 **DeployToDatabase**プロパティは**False**を指定して **/dd-** またはスイッチは省略、VSDBCMD は、データベースは展開しません。
- 場合、 **DeployToDatabase**プロパティは**True**VSDBCMD は、スイッチを無視して、データベースを展開します。
- データベースを配置するかどうかに関係なく、各ケースでは、配置スクリプトが生成されます。

## <a name="conclusion"></a>まとめ

このトピックでは、Visual Studio 2010 データベース プロジェクトのビルドと展開プロセスの概要が提供されます。 また、エンタープライズ規模のデータベースの配置をサポートするために、MSBuild で VSDBCMD.exe を使用する方法も説明します。

実際にこのしくみの詳細については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)します。

## <a name="further-reading"></a>関連項目

環境ごとに個別のデプロイの構成ファイルを作成してデータベースの配置をカスタマイズする方法については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)します。 配置後スクリプトを実行してデータベース ロールのメンバーシップを構成する方法のガイダンスについては、次を参照してください。[テスト環境にデータベース ロール メンバーシップを展開する](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)します。 固有の課題のいくつかのメンバーシップを管理する方法のガイダンスについては、データベースは強制を参照してください[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)します。

MSDN でこれらのトピックより広範なガイダンスと背景情報については、Visual Studio データベース プロジェクトとデータベースの展開プロセスを説明します。

- [Visual Studio 2010 の SQL Server データベース プロジェクト](https://msdn.microsoft.com/library/ff678491.aspx)
- [データベースの変更を管理します。](https://msdn.microsoft.com/library/aa833404.aspx)
- [方法: VSDBCMD を使用して、コマンド プロンプトからの展開データベースを準備します。実行可能ファイル](https://msdn.microsoft.com/library/dd193258.aspx)
- [データベースのビルドと展開の概要](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [前へ](deploying-web-packages.md)
> [次へ](creating-and-running-a-deployment-command-file.md)
