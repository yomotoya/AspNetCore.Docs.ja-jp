---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: ビルド プロセスを理解する |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、エンタープライズ規模のビルドおよび配置プロセスのチュートリアルを提供します。 このトピックで説明されているアプローチは、カスタムの Microsoft ビルド エンジニアを使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-the-build-process"></a>ビルド プロセスの理解
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、エンタープライズ規模のビルドおよび配置プロセスのチュートリアルを提供します。 このトピックで説明されているアプローチは、プロセスのすべての側面の詳細に制御を提供するのにカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用します。 プロジェクト ファイル内では、カスタム MSBuild ターゲットは、インターネット インフォメーション サービス (IIS) Web 配置ツール (MSDeploy.exe) などの展開ユーティリティおよび VSDBCMD.exe データベース配置ユーティリティを実行に使用されます。
> 
> > [!NOTE]
> > 前のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)MSBuild プロジェクト ファイルの主要なコンポーネントの説明、および複数のターゲット環境に展開をサポートするプロジェクト ファイルの分割の概念が導入されました。 わからない場合既にこれらの概念を理解して、確認してください[プロジェクト ファイルを理解する](understanding-the-project-file.md)このトピックの内容を実行する前にします。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="build-and-deployment-overview"></a>ビルドと展開の概要

[Contact Manager ソリューション](the-contact-manager-solution.md)、3 つのファイルがビルドおよび配置プロセスを制御します。

- A*ユニバーサル プロジェクト ファイル*(*Publish.proj*)。 これには、移行先の環境間で変更されないビルドおよび展開の説明が含まれています。
- *環境固有のプロジェクト ファイル*(*Env Dev.proj*)。 これには、特定の送信先の環境に固有のビルドと配置の設定が含まれています。 たとえば、使用する、 *Env Dev.proj*開発者またはテスト環境の設定を入力し、という代替ファイルを作成するファイル*Env Stage.proj*ステージングの設定を指定環境。
- A*コマンド ファイル*(*発行 Dev.cmd*)。 これには、プロジェクト ファイルを指定するコマンドを実行する場合、MSBuild.exe が含まれています。 ファイルごとに別の環境に固有のプロジェクト ファイルを指定する MSBuild.exe コマンドが含まれています、移行先環境ごとにコマンド ファイルを作成することができます。 開発者はでき、適切なコマンド ファイルを実行するだけで、別の環境に展開します。

サンプル ソリューションでは、発行ソリューション フォルダーにこれら 3 つのファイルを検索できます。

![](understanding-the-build-process/_static/image1.png)

これらのファイルをさらに詳しく見ると、前にこのアプローチを使用するときの全体的なビルド処理のしくみを見てをみましょう。 大まかに言えば、次のようなビルドおよび配置プロセス。

![](understanding-the-build-process/_static/image2.png)

最初に行われるは、2 つのプロジェクト ファイル&#x2014;と環境固有の設定を含む 1 つの汎用のビルドと配置の手順を含む&#x2014;1 つのプロジェクト ファイルにマージされます。 MSBuild は、プロジェクト ファイル内の命令を動作します。 各ソリューションでは、各プロジェクトのプロジェクト ファイルを使用してプロジェクトを作成します。 するために呼び出す Web Deploy (MSDeploy.exe) など、他のツールと、web コンテンツやデータベースを対象となる環境に展開する VSDBCMD ユーティリティです。

開始から終了までは、ビルドおよび配置プロセスは、これらのタスクを実行します。

1. 新しいビルドの準備として、出力ディレクトリの内容を削除します。
2. ソリューション内の各プロジェクトを作成します。

    1. Web プロジェクトの&#x2014;ここでは、ASP.NET MVC web アプリケーションと WCF web サービス&#x2014;ビルド処理は、各プロジェクトの web 配置パッケージを作成します。
    2. データベース プロジェクトの場合は、ビルド プロセスは、各プロジェクトの配置マニフェスト (.deploymanifest ファイル) を作成します。
3. VSDBCMD.exe ユーティリティを使用して、プロジェクト ファイルからさまざまなプロパティを使用して、ソリューション内の各データベース プロジェクトを配置を&#x2014;ターゲット接続文字列とデータベース名の&#x2014;.deploymanifest ファイルと共にします。
4. これは、MSDeploy.exe ユーティリティを使用して、ソリューションでは、プロジェクト ファイルからのさまざまなプロパティを使用して、展開プロセスを制御するには、各 web プロジェクトを配置します。

サンプル ソリューションを使用すると、このプロセスの詳細をトレースします。

> [!NOTE]
> サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。


## <a name="invoking-the-build-and-deployment-process"></a>ビルドと展開プロセスの呼び出し

開発者が実行に連絡先のマネージャー ソリューション開発者のテスト環境を配置するのには、*発行 Dev.cmd*コマンド ファイルです。 MSBuild.exe を起動を指定する*Publish.proj* 、プロジェクト ファイルを実行すると*Env Dev.proj*パラメーターの値として。


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl**スイッチ (の略 **/fileLogger**) ビルドの出力をという名前のファイルに記録*msbuild.log になります*現在のディレクトリにします。 詳細については、次を参照してください。、 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。


この時点では、MSBuild の実行を開始、ロード、 *Publish.proj*ファイル、およびその中の手順の処理を開始します。 最初の命令が MSBuild プロジェクトをインポートするように指示ファイルを**TargetEnvPropsFile**パラメーターを指定します。


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile**パラメーターを指定します、 *Env Dev.proj*ファイル、MSBuild の内容をマージするため、 *Env Dev.proj*ファイルを*Publish.proj*ファイル。

MSBuild がマージされたプロジェクト ファイルで発生する次の要素は、プロパティのグループです。 プロパティは、ファイル内に出現する順序で処理されます。 MSBuild では、指定した条件を満たしていることを提供する各プロパティのキー/値ペアを作成します。 後で定義されているファイルのプロパティ ファイルで以前に定義されている同じ名前を持つ任意のプロパティが上書きされます。 たとえば、 **OutputRoot**プロパティです。


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


MSBuild が 1 つ目を処理するときに**OutputRoot**要素、同様に名前付きパラメーターを提供することが指定されていませんの値を設定、 **OutputRoot**プロパティを **.\Publish\Out**です。2 つ目が出現したとき**OutputRoot**に条件が評価された場合、要素**true**の値は、上書きされます、 **OutputRoot**プロパティの値と、**OutDir**パラメーター。

MSBuild が発生した次の要素は、という名前の項目を含む、1 つの項目グループ**ProjectsToBuild**です。


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild では、この命令を処理という名前の項目リストを構築して**ProjectsToBuild**です。 ここでは、項目リストが 1 つの値が含まれます&#x2014;ソリューション ファイルのファイル名とパス。

この時点では、残りの要素は、ターゲットです。 プロパティと項目からターゲットを異なる方法で処理されます&#x2014;明示的にユーザーによって指定またはされるプロジェクト ファイル内の別の構成体によって呼び出された場合を除き、基本的には、ターゲットは処理されません。 注意してください、opening**プロジェクト**タグが含まれています、 **DefaultTargets**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


これは、msbuild でを呼び出す、 **FullPublish**ターゲット、ターゲットがない場合は、MSBuild.exe が呼び出される場合を指定します。 **FullPublish**ターゲットにはすべてのタスクが含まれていない代わりに単を指定します。 依存関係の一覧です。


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


この依存関係が MSBuild をさせるにを実行するように指示、 **FullPublish**ターゲットをこの順でターゲットの一覧を呼び出す必要があります。

1. これを呼び出す必要があります、**クリーン**ターゲットです。
2. これを呼び出す必要があります、 **BuildProjects**ターゲットです。
3. これを呼び出す必要があります、 **GatherPackagesForPublishing**ターゲットです。
4. これを呼び出す必要があります、 **PublishDbPackages**ターゲットです。
5. これを呼び出す必要があります、 **PublishWebPackages**ターゲットです。

### <a name="the-clean-target"></a>Clean ターゲット

**クリーン**ターゲット基本的には、出力ディレクトリとそのすべての内容として削除新しくビルドを準備します。


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


ターゲットを含む通知、 **ItemGroup**要素。 プロパティまたは内の項目を定義する場合、**ターゲット**要素を作成する*動的*プロパティと項目。 つまり、プロパティまたは項目は、ターゲットが実行されるまでに処理されません。 出力ディレクトリが存在しないか、ビルドすることはできませんので、ビルド プロセスが開始されるまで、ファイルを含める、  **\_FilesToDelete**静的アイテムとして一覧以外の実行は進行するまで待機する必要があります。 そのため、リストを作成するには、ターゲット内の動的アイテムとして。

> [!NOTE]
> この場合、ため、**クリーン**ターゲットは、最初に実行される、実際には動的なグループを使用する必要はありません。 ただし、お勧めこの種類のシナリオでは、動的なプロパティと項目を使用するようにいくつかの時点では異なる順序でターゲットを実行することができます。  
> 使用しないアイテムを宣言することを回避する目指す必要があります。 特定のターゲットでのみ使用される項目をある場合は、ビルド プロセスで、不要なオーバーヘッドを削除する対象の内側に配置することを検討してください。


項目を別に、動的、**クリーン**ターゲットが簡単にし、組み込みの利用**メッセージ**、**削除**、および**RemoveDir**へのタスクします。

1. ロガーにメッセージを送信します。
2. 削除するファイルの一覧を作成します。
3. ファイルを削除します。
4. 出力ディレクトリを削除します。

### <a name="the-buildprojects-target"></a>BuildProjects ターゲット

**BuildProjects**ターゲットは基本的には、サンプル ソリューション内のすべてのプロジェクトをビルドします。


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


このターゲットは、前のトピックでは、ある程度詳細に説明した[プロジェクト ファイルを理解する](understanding-the-project-file.md)タスクとコア ターゲット プロパティと項目の参照方法を説明するためにします。 この時点では、主に関心、 **MSBuild**タスク。 このタスクを使用すると、複数のプロジェクトをビルドします。 タスクが MSBuild.exe; の新しいインスタンスを作成できません。各プロジェクトをビルドするのに現在の実行中のインスタンスを使用します。 この例では関心のある重要な点は、展開プロパティを示します。

- **DeployOnBuild**プロパティには、各プロジェクトのビルドが完了すると、プロジェクトの設定で、デプロイの手順を実行する MSBuild がよう指示します。
- **DeployTarget**プロパティは、プロジェクトをビルドした後、呼び出し先のターゲットを識別します。 ここで、**パッケージ**ターゲットが配置可能な web のパッケージにプロジェクトの出力をビルドします。

> [!NOTE]
> **パッケージ**ターゲット呼び出して、Web 発行パイプライン (WPP)、MSBuild および Web デプロイの間の統合を提供します。 見て確認、組み込みターゲット、WPP が提供する場合、 *Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダー内のファイルです。


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing ターゲット

調査する場合、 **GatherPackagesForPublishing**ターゲットわかりますを実際にが含まれていないすべてのタスクです。 代わりに、3 つの動的な項目を定義する 1 つの項目グループが含まれています。


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


これらの項目は、ときに作成された展開パッケージを参照してください、 **BuildProjects**ターゲットが実行されました。 できませんでした。 これらの項目を静的に定義プロジェクト ファイルでまでの項目が参照するファイルが存在しないため、 **BuildProjects**ターゲットを実行します。 代わりに、項目必要がありますに動的に内で定義するまでは呼び出されませんターゲット後、 **BuildProjects**ターゲットを実行します。

このターゲット内の項目は使用されません&#x2014;項目と各項目の値に関連付けられているメタデータは、このターゲットを単にビルドします。 これらの要素を処理した後、 **PublishPackages**項目が 2 つの値へのパスを含む、 *ContactManager.Mvc.deploy.cmd*ファイルとパスを*ContactManager.Service.deploy.cmd*ファイル。 Web Deploy は、各プロジェクトの web パッケージの一部としてこれらのファイルを作成し、これらは、呼び出す必要のあるファイル、パッケージを展開するために、移行先サーバーにします。 これらのファイルのいずれかを開くには場合、基本的にはさまざまなビルドに固有のパラメーター値を持つ、MSDeploy.exe コマンドが表示されます。

**DbPublishPackages**項目は、単一の値へのパスを含む、 *ContactManager.Database.deploymanifest*ファイル。

> [!NOTE]
> データベース プロジェクトをビルドして、MSBuild プロジェクト ファイルと同じスキーマを使用して .deploymanifest ファイルが生成されます。 データベース スキーマ (.dbschema) の場所と任意の配置前や配置後スクリプトの詳細を含む、データベースを展開するために必要なすべての情報が含まれています。 詳細については、次を参照してください。 [、概要のデータベースのビルドと配置](https://msdn.microsoft.com/library/aa833165.aspx)です。


展開パッケージとデータベースの配置マニフェストの作成およびの使用方法の詳細を学習[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)と[データベース プロジェクトの配置](deploying-database-projects.md)です。

### <a name="the-publishdbpackages-target"></a>PublishDbPackages ターゲット

簡単に言うと、 **PublishDbPackages**ターゲットは、展開する VSDBCMD ユーティリティを呼び出して、 **ContactManager**をターゲット環境にデータベース。 決定および深入りの多くは、データベースの配置を構成して、これにはの詳細を学習[データベース プロジェクトの配置](deploying-database-projects.md)と[の複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). このトピックでは、このターゲットが実際に機能する方法に注目します。

最初に、開始タグが含まれていることを確認、**出力**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


これは、例の*ターゲットのバッチ*です。 MSBuild プロジェクト ファイルでは、バッチ処理は、コレクションを反復処理するための手法です。 値、**出力**属性、 **「% (DbPublishPackages.Identity)」** を参照、 **Identity**のメタデータのプロパティ、 **DbPublishPackages**項目のリスト。 この表記 **Outputs=%***(ItemList.ItemMetadataName)*、として変換されます。

- 内の項目を分割**DbPublishPackages**同じが含まれる項目のバッチに**Identity**メタデータ値。
- バッチごとに 1 回のターゲットが実行されます。

> [!NOTE]
> **Identity**の 1 つ、[組み込みメタデータ値](https://msdn.microsoft.com/library/ms164313.aspx)作成時にすべての項目に割り当てられています。 値を参照して、 **Include**属性、**項目**要素&#x2014;言い換えれば、パスとファイル名は、項目の。


この場合、同じパスとファイル名を持つ 2 つ以上の項目することはありません必要があります、本質的に取り組んでいますに 1 つのバッチ サイズ。 ターゲットは、データベース パッケージごとに 1 回実行します。

同様の表記法を表示できます、  **\_Cmd**プロパティで、適切なスイッチ VSDBCMD コマンドを構築します。


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


この場合、 **%(DbPublishPackages.DatabaseConnectionString)**、 **%(DbPublishPackages.TargetDatabase)**、および **%(DbPublishPackages.FullPath)** すべてを参照してくださいメタデータの値、 **DbPublishPackages**項目のコレクション。 **\_Cmd**プロパティを使用、 **Exec**タスクは、コマンドを呼び出します。


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


この表記では、結果として、 **Exec**タスクの一意の組み合わせに基づいて、バッチが作成されます、 **DatabaseConnectionString**、 **TargetDatabase**、および**FullPath**メタデータ値、およびタスクは、バッチごとに 1 回実行します。 これは、例の*タスクのバッチ*です。 ただし、ターゲット レベルのバッチ処理が既に分割されている、項目のコレクションを単一項目のバッチにあるため、 **Exec**タスクは、1 回とターゲットの繰り返しごとに 1 回だけ実行されます。 つまり、このタスクは、ソリューション内の各データベース パッケージを 1 回 VSDBCMD ユーティリティを呼び出します。

> [!NOTE]
> ターゲットとタスクのバッチの詳細については、MSBuild を参照してください。[バッチ処理](https://msdn.microsoft.com/library/ms171473.aspx)、[ターゲットのバッチ内の項目メタデータ](https://msdn.microsoft.com/library/ms228229.aspx)、および[タスクのバッチ内の項目メタデータ](https://msdn.microsoft.com/library/ms171474.aspx)です。


### <a name="the-publishwebpackages-target"></a>PublishWebPackages ターゲット

この段階では、呼び出した、 **BuildProjects**ターゲットで、サンプル ソリューションの各プロジェクトの web 配置パッケージが生成されます。 各パッケージに付属する、 *deploy.cmd*を対象となる環境にパッケージを展開に必要な MSDeploy.exe コマンドを含む、ファイルと*SetParameters.xml*を指定するファイル、ターゲット環境のために必要な詳細です。 呼び出したも、 **GatherPackagesForPublishing**ターゲットを含む項目コレクションが生成されますが、 *deploy.cmd*ファイルに関心があります。 基本的には、 **PublishWebPackages**ターゲットは、これらの関数を実行します。

- 操作し、 *SetParameters.xml*ターゲット環境の適切な詳細を含めるには、各パッケージのファイルを使用して、 **XmlPoke**タスク。
- 呼び出す、 *deploy.cmd*適切なスイッチを使用して、各パッケージ用のファイルです。

同じように、 **PublishDbPackages** 、ターゲット、 **PublishWebPackages**ターゲットでターゲットのバッチを使用して、ターゲットは web パッケージごとに 1 回実行します。


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


ターゲット内、 **Exec**タスクが実行に使用される、 *deploy.cmd* web パッケージごとにファイル。


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Web パッケージの展開を構成する方法については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。

## <a name="conclusion"></a>まとめ

このトピックでは、分割されたプロジェクト ファイルを使用して開始から終了連絡先のマネージャーのサンプル ソリューションのビルドおよび配置プロセスを制御する方法のチュートリアルが用意されています。 このアプローチを使用すると、環境固有のコマンド ファイルを実行するだけで実行する複合型、1 つに、反復可能な手順で、エンタープライズ規模の展開ができます。

## <a name="further-reading"></a>関連項目

プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。

> [!div class="step-by-step"]
> [前へ](understanding-the-project-file.md)
> [次へ](building-and-packaging-web-application-projects.md)
