---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: データベース プロジェクトの配置 |Microsoft ドキュメント
author: jrjlee
description: '注: エンタープライズ展開シナリオの多くは、する必要が配置されたデータベースに増分更新を公開する機能。 代替手段は、再作成には.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882502"
---
<a name="deploying-database-projects"></a>データベース プロジェクトの配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> エンタープライズ展開シナリオの多くは、配置されたデータベースに増分更新を公開する機能が必要です。 代わりに、すべての展開では、既存のデータベース内のデータが失われることを意味上のデータベースを再作成を開始します。 Visual Studio 2010 を使用する場合は、増分データベース発行する方法をお勧めは VSDBCMD を使用します。 ただし、Visual Studio と Web 発行パイプライン (WPP) の次のバージョンを増分直接公開をサポートするツールが含まれます。


Visual Studio 2010 での連絡先のマネージャーのサンプル ソリューションを開いた場合、[プロパティ] フォルダーに 4 つのファイルが含まれていますが、データベース プロジェクトに含まれているが表示されます。

![](deploying-database-projects/_static/image1.png)

プロジェクト ファイルと共に (*ContactManager.Database.dbproj*ここでは)、これらのファイルがビルドおよび配置プロセスのさまざまな側面を制御します。

- *Database.sqlcmdvars*ファイルは、プロジェクトを配置するときに使用するすべての SQLCMD 変数の値を提供します。 各ソリューションの構成 (たとえば、デバッグやリリースなど) は、異なる .sqlcmdvars ファイルを指定できます。
- *Database.sqldeployment*ファイル、プロジェクトまたは移行先サーバーの照合順序で定義された照合順序を使用するかどうかのように、展開に固有の設定は、転送先データベースを再作成するかどうかすべて時間または単に最新の状態を表示するために既存のデータベースを修正します。 各ソリューション構成では、異なる .sqldeployment ファイルを指定できます。
- *Database.sqlpermissions*ファイルは、ターゲット データベースに追加する任意の権限の定義に使用できる XML ドキュメントです。 すべてのソリューション構成では、同じ .sqlpermissions ファイルを共有します。
- *Database.sqlsettings*ファイルが、比較演算子の動作を使用する照合順序と同様に、データベースを作成するときに使用するデータベース レベルのプロパティを指定します。 すべてのソリューション構成では、同じ .sqlsettings ファイルを共有します。

ですが、Visual Studio でこれらのファイルを開き、内容を理解します。

データベース プロジェクトをビルドすると、ビルド処理には、2 つのファイルが作成されます。

- A*データベース スキーマ*(.dbschema ファイル)。 これには、XML 形式で作成するデータベースのスキーマについて説明します。
- A*配置マニフェスト*(.deploymanifest ファイル)。 これには、作成し、データベースを展開するために必要なすべての情報が含まれています。 デプロイの手順 (.sqldeployment ファイル) と、配置前や配置後 SQL スクリプトのように、他のリソースと一緒に .dbschema ファイルを参照します。

これは、これらのリソース間の関係を示しています。

![](deploying-database-projects/_static/image2.png)

わかるように、.sqlsettings ファイルおよび .sqlpermissions ファイルは、ビルド処理への入力します。 データベース プロジェクト ファイルと共に、これらのファイルは、データベースのスキーマ ファイルを作成に使用されます。 .Sqldeployment ファイルおよび .sqlcmdvars ファイルは、変更、ビルド プロセスを通過します。 配置マニフェストでは、データベース スキーマ、.sqldeployment ファイル、.sqlcmdvars ファイルおよび任意の配置前や配置後 SQL スクリプトの場所を示します。

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>データベース プロジェクトを配置するのに VSDBCMD を使用する理由

データベース プロジェクトを配置するさまざまなさまざまな方法はあります。 ただし、すべてでは、エンタープライズ環境でのリモート サーバーにデータベース プロジェクトを展開するために適しています。 データベース プロジェクトの配置からすることを検討してください。 エンタープライズ展開シナリオで表示することを選択します。

- リモートの場所からデータベース プロジェクトを配置する権限です。
- 既存のデータベースを増分更新する権限です。
- 配置前スクリプトまたは配置後スクリプトを含める機能。
- 複数の展開先環境への配置を調整する権限です。
- 大きなの一部としてデータベース プロジェクトを配置する機能通常、スクリプト化された 1 つの手順から成るソリューションの配置。

データベース プロジェクトを展開に使用できる 3 つの主な方法はあります。

- Visual Studio 2010 データベース プロジェクトの種類の展開機能を使用することができます。 ビルド Visual Studio 2010 データベース プロジェクトを配置すると、展開プロセスは、ビルド構成に固有の SQL ベースの展開ファイルを生成するのに、配置マニフェストを使用します。 既に存在かが既に存在する場合は、データベースに必要な変更を加える場合、データベースが作成されます。 SQLCMD.exe を使用するには、移行先サーバーでこのファイルを実行するか、作成して、ファイルを実行する Visual Studio を設定することができます。 この方法の欠点は、展開の設定の制御がのみに制限されることです。 多くの場合も、ファイルを変更する、SQL 配置、特定の環境変数の値を指定する必要があります。 Visual Studio 2010 がインストールされている場合、この方法は、コンピューターからのみ使用でき、開発者は、調べることや、すべての展開先環境の接続文字列と資格情報を提供する必要があります。
- インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用することができます[web アプリケーション プロジェクトの一部としてデータベースを展開](https://msdn.microsoft.com/library/dd465343.aspx)です。 ただし、この手法は、単に移行先サーバー上の既存のローカル データベースをレプリケートするのではなく、データベース プロジェクトを配置する場合、もっと複雑です。 Web 配置を行うにが、データベース プロジェクトを生成する SQL 配置スクリプトを実行することができます、カスタム WPP ターゲット ファイル、web アプリケーション プロジェクトを作成する必要があります。 展開プロセスの複雑さが大量に追加します。 さらに、Web Deploy は直接できません既存のデータベースの増分更新します。 この方法の詳細については、次を参照してください。 [SQL ファイルを展開パッケージのデータベース プロジェクトに Web 発行パイプラインを拡張する](https://go.microsoft.com/?linkid=9805121)です。
- VSDBCMD ユーティリティを使用して、データベース スキーマまたは配置マニフェストを使用して、データベースを配置することができます。 スクリプト化されたより大規模の展開プロセスの一部としてデータベースをパブリッシュすることができる MSBuild ターゲットから VSDBCMD.exe を呼び出すことができます。 .Sqlcmdvars ファイルおよびその他のデータベースのプロパティを複数のビルド構成を作成せず、展開のさまざまな環境をカスタマイズすることができます、VSDBCMD のコマンドから多数の変数を上書きすることができます。 VSDBCMD は、データベース スキーマを転送先データベースを配置するために必要な変更のみを行うことを意味、差別化機能を提供します。 VSDBCMD には、さまざまなコマンド ライン オプションは、展開プロセスをよりきめ細かく制御も提供しています。

この概要から MSBuild を使用して VSDBCMD を使用するが一般的なエンタープライズ展開シナリオに最適な方法を確認できます。

|  | Visual Studio 2010 | Web 配置 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| リモート展開をサポートしますか。 | [はい] | はい | [はい] |
| 増分更新をサポートしますか。 | [はい] | いいえ | [はい] |
| 前または配置後のスクリプトをサポートしていますか。 | [はい] | はい | [はい] |
| 複数の環境の展開をサポートしていますか。 | 制限 | 制限 | [はい] |
| スクリプト化された展開をサポートしていますか。 | 制限 | [はい] | [はい] |

このトピックの残りの部分では、データベース プロジェクトを配置する MSBuild を使用して VSDBCMD の使用について説明します。

## <a name="understanding-the-deployment-process"></a>展開プロセスを理解します。

VSDBCMD ユーティリティを使用して、データベース スキーマ (.dbschema ファイル) または配置マニフェスト (.deploymanifest ファイル) を使用してデータベースを展開できます。 実際には、ほぼ常に使用する、配置マニフェストと配置マニフェストでは、さまざまな展開のプロパティの既定値を指定し、配置前や配置後 SQL スクリプトを実行するを特定することができます。 展開するこの VSDBCMD コマンドを使用するなど、 **ContactManager**データベースをテスト環境でのデータベース サーバーにします。


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


この場合、次のようになります。

- **/A** (または **/Action**) を行うには VSDBCMD するスイッチを指定します。 これを設定することができます**インポート**または**展開**です。 **インポート**オプションを使用して、既存のデータベースから .dbschema ファイルを生成し、**展開**.dbschema ファイルをターゲット データベースに展開するオプションを使用します。
- **/Manifest** (または **/ManifestFile**) スイッチを展開する .deploymanifest ファイル名を指定します。 代わりに、.dbschema ファイルを使用する場合は、使用、**モデル/** (または **/ModelFile**) スイッチします。
- **/Cs** (または **/ConnectionString**) スイッチが対象のデータベース サーバーの接続文字列を提供します。 データベースの名前は含まれませんことに注意してください&#x2014;VSDBCMD がデータベースを作成するサーバーに接続する必要があります個々 のデータベースに接続する必要はありません。 .Deploymanifest ファイルには、接続文字列が含まれている場合は、このスイッチを省略できます。 いずれにしても、スイッチを使用する場合、スイッチの値は .deploymanifest 値をオーバーライドします。
- <strong>/P:TargetDatabase</strong>プロパティをターゲット データベースの作成時に割り当てる名前を提供します。 値が上書きされます。、 <strong>TargetDatabase</strong> .deploymanifest ファイル内のプロパティです。 使用することができます、 <strong>/p:</strong> <em>[プロパティ名]</em>.sqlcmdvars ファイルで宣言されている構文をさまざまな展開のプロパティを設定して、すべての SQLCMD 変数を上書きします。
- **/Dd+** (または **/DeployToDatabase+**) スイッチでは、展開を作成し、ターゲット環境に展開することを示します。 指定した場合 **/dd-** スイッチは省略または、VSDBCMD は配置スクリプトが生成されますが、ターゲット環境に配置されません。 このスイッチは、混乱の原因は、多くの場合とは、次のセクションで詳しく説明します。
- **/Script** (または **/DeploymentScriptFile**) スイッチでは、配置スクリプトを生成する場所を指定します。 この値は、展開プロセスには影響しません。

VSDBCMD の詳細については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンスです。EXE (配置とスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)と[する方法: VSDBCMD を使用して、コマンド プロンプトからの展開をデータベースを準備します。EXE](https://msdn.microsoft.com/library/dd193258.aspx)です。

MSBuild プロジェクト ファイルから VSDBCMD を使用する方法の例は、次を参照してください。[ビルド プロセスの理解](understanding-the-build-process.md)です。 複数の環境のデータベースのデプロイ設定を構成する方法の例については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)です。

## <a name="understanding-the-deploytodatabase-switch"></a>DeployToDatabase スイッチを理解します。

動作、 **/dd**または **/DeployToDatabase**スイッチは、VSDBCMD .dbschema ファイルまたは .deploymanifest ファイルで使用するかどうかによって異なります。 .Dbschema ファイルを使用している場合の動作は非常に簡単です。

- 指定した場合 **/dd+** または **/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。
- 指定した場合 **/dd-** またはスイッチは省略、VSDBCMD のみ配置スクリプトが生成されます。

.Deploymanifest ファイルを使用している場合の動作ははるかに複雑です。 これは .deploymanifest ファイルには、プロパティ名が含まれる**DeployToDatabase**もデータベースが配置されたかどうかを決定します。


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


このプロパティの値は、データベース プロジェクトのプロパティに従って設定されています。 設定した場合、**配置アクション**に**展開スクリプト (.sql) を作成する**、値になります**False**です。 設定した場合、**配置アクション**に**展開スクリプト (.sql) を作成し、データベースに展開**、値になります**True**です。

> [!NOTE]
> これらの設定は、特定のビルド構成とプラットフォームに関連付けられます。 設定を構成する場合など、**デバッグ**構成し、発行を使用して、**リリース**構成では、設定は使用されません。


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> このシナリオでは、**配置アクション**に常に設定する必要があります**展開スクリプト (.sql) を作成する**Visual Studio 2010、データベースの配置をしたくないので、します。 言い換えると、 **DeployToDatabase**プロパティは常に**False**です。


ときに、 **DeployToDatabase**プロパティが指定されて、 **/dd**プロパティの値がある場合は、スイッチに、プロパティはオーバーライドのみ**false**:

- 場合、 **DeployToDatabase**プロパティは**False**を指定して **/dd+** または **/dd**、VSDBCMD よりも優先されます、 **DeployToDatabase**プロパティおよびデータベースを展開します。
- 場合、 **DeployToDatabase**プロパティは**False**を指定して **/dd-** またはスイッチは省略、VSDBCMD データベースに展開できなくなります。
- 場合、 **DeployToDatabase**プロパティは**True**VSDBCMD は、スイッチは無視され、データベースを展開します。
- データベースを配置するかどうかに関係なく、各ケースでは、配置スクリプトが生成されます。

## <a name="conclusion"></a>まとめ

このトピックでは、Visual Studio 2010 データベース プロジェクトのビルドおよび配置プロセスの概要が用意されています。 また、エンタープライズ規模のデータベースの配置をサポートするために、MSBuild で VSDBCMD.exe を使用する方法も説明します。

実際にこのしくみの詳細については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)です。

## <a name="further-reading"></a>関連項目

環境ごとに個別の配置構成ファイルを作成してデータベースの配置をカスタマイズする方法については、次を参照してください。[複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md)です。 配置後スクリプトを実行してデータベース ロール メンバーシップを構成する方法のガイダンスについては、次を参照してください。[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。 ガイダンスについては、そのメンバーシップを管理する固有の課題の一部のデータベースを示しているを参照してください[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)です。

MSDN の以下のトピックより広範なガイダンスと背景情報については、Visual Studio データベース プロジェクトおよびデータベースの展開プロセスが提供します。

- [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx)
- [データベースの変更を管理します。](https://msdn.microsoft.com/library/aa833404.aspx)
- [方法: VSDBCMD を使用して、コマンド プロンプトからの展開をデータベースを準備します。EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [データベースのビルドと配置の概要](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [前へ](deploying-web-packages.md)
> [次へ](creating-and-running-a-deployment-command-file.md)
