---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: ビルド プロセスの理解 |Microsoft Docs
author: jrjlee
description: このトピックでは、エンタープライズ規模のビルドと展開プロセスのチュートリアルを示します。 このトピックで説明したアプローチでは、Microsoft のカスタム ビルド エンジニアを使用しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 9df145b281b086f546c55d0b26a8b0e44e896bb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837395"
---
<a name="understanding-the-build-process"></a>ビルド プロセスを理解します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、エンタープライズ規模のビルドと展開プロセスのチュートリアルを示します。 このトピックで説明した方法では、プロセスのすべての側面の詳細に制御を提供するのにカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用します。 プロジェクト ファイル内では、カスタム MSBuild のターゲットは、インターネット インフォメーション サービス (IIS) Web 配置ツール (MSDeploy.exe) などの展開ユーティリティと VSDBCMD.exe データベース配置ユーティリティの実行に使用されます。
> 
> > [!NOTE]
> > 前のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)、MSBuild プロジェクト ファイルの主要なコンポーネントを説明し、複数のターゲット環境に展開をサポートするプロジェクト ファイルの分割の概念が導入されました。 確認する必要がありますなら既にこれらの概念について、[プロジェクト ファイルを理解する](understanding-the-project-file.md)前に、このトピックで作業します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="build-and-deployment-overview"></a>ビルドと展開の概要

[連絡先マネージャー ソリューション](the-contact-manager-solution.md)、3 つのファイルがビルドおよび配置プロセスを制御します。

- A*ユニバーサル プロジェクト ファイル*(*Publish.proj*)。 これには、移行先の環境間で変更されないビルドおよび展開の説明が含まれています。
- *環境固有のプロジェクト ファイル*(*Env Dev.proj*)。 これには、特定の送信先の環境に固有のビルドと配置の設定が含まれています。 たとえば、使用する、 *Env Dev.proj*開発者またはテスト環境の設定を行い、という名前の別のファイルを作成するファイル*Env Stage.proj*ステージングの設定を指定環境。
- A*コマンド ファイル*(*発行 Dev.cmd*)。 これには、プロジェクト ファイルを指定するコマンドを実行する場合、MSBuild.exe が含まれています。 各ファイルが別の環境に固有のプロジェクト ファイルを指定する MSBuild.exe のコマンドが含まれているすべての変換先の環境のコマンド ファイルを作成できます。 これにより、開発者が適切なコマンド ファイルを実行するだけでさまざまな環境にデプロイできます。

サンプルのソリューションでは、これら 3 つの発行フォルダーにソリューション ファイルが見つかります。

![](understanding-the-build-process/_static/image1.png)

これらのファイルをさらに詳しく見ると、前にこのアプローチを使用する場合の全体的なビルド プロセスのしくみを見てをみましょう。 大まかに言えば、ビルドおよび配置プロセスはようになります。

![](understanding-the-build-process/_static/image2.png)

最初に行われるは、2 つのプロジェクト ファイル&#x2014;ユニバーサルのビルドと配置の手順を含む 1 つ、1 つの環境に固有の設定を含む&#x2014;1 つのプロジェクト ファイルにマージされます。 MSBuild はプロジェクト ファイル内の命令を動作します。 各プロジェクトごとに、プロジェクト ファイルを使用して、ソリューションでプロジェクトを構築します。 これは、後に、Web Deploy (MSDeploy.exe) などの他のツールと、web コンテンツおよびデータベースを対象となる環境にデプロイする VSDBCMD ユーティリティ呼び出します。

[完了] を [スタート] からは、ビルドおよび配置プロセスは、これらのタスクを実行します。

1. 新しいビルドの準備として、出力ディレクトリの内容を削除します。
2. ソリューション内の各プロジェクトをビルドします。

    1. Web プロジェクトの&#x2014;ここでは、ASP.NET MVC web アプリケーションと WCF web サービス&#x2014;ビルド プロセスは、各プロジェクトの web 展開パッケージを作成します。
    2. データベース プロジェクトの場合は、ビルド プロセスは、各プロジェクトの配置マニフェスト (.deploymanifest ファイル) を作成します。
3. VSDBCMD.exe ユーティリティを使用して、プロジェクト ファイルからさまざまなプロパティを使用して、ソリューション内の各データベース プロジェクトのデプロイを&#x2014;ターゲット接続文字列とデータベース名を&#x2014;.deploymanifest ファイルと共にします。
4. MSDeploy.exe ユーティリティを使用して、プロジェクト ファイルから展開プロセスを制御するさまざまなプロパティを使用して、ソリューション内の各 web プロジェクトをデプロイします。

サンプル ソリューションを使用して、このプロセスの詳細をトレースすることができます。

> [!NOTE]
> 独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。


## <a name="invoking-the-build-and-deployment-process"></a>ビルドと展開プロセスを呼び出す

開発者が実行連絡先マネージャー ソリューションを開発者のテスト環境をデプロイするのには、*発行 Dev.cmd*コマンド ファイルです。 MSBuild.exe を起動を指定する*Publish.proj*として実行するプロジェクト ファイルと*Env Dev.proj*パラメーター値として。


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> **/Fl**スイッチ (短縮形 **/fileLogger**) という名前のファイルのビルド出力をログに記録*msbuild.log になります*現在のディレクトリで。 詳細については、次を参照してください。、 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)します。


この時点では、MSBuild の実行を開始、読み込み、 *Publish.proj*ファイル、およびその中の手順の処理を開始します。 最初の命令が MSBuild プロジェクトをインポートするように指示するファイル、 **TargetEnvPropsFile**パラメーターを指定します。


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


**TargetEnvPropsFile**パラメーターを指定します、 *Env Dev.proj*ファイル、MSBuild の内容をマージするため、 *Env Dev.proj*ファイルを*Publish.proj*ファイル。

MSBuild は、マージされたプロジェクト ファイル内で検出された次の要素は、プロパティ グループです。 プロパティは、ファイルに出現する順序で処理されます。 MSBuild は、指定した条件を満たしていることを提供する、各プロパティのキー/値ペアを作成します。 後で定義されているファイルのプロパティと同じ名前のファイルで既に定義済みプロパティが上書きされます。 たとえば、 **OutputRoot**プロパティ。


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


MSBuild が最初に処理と**OutputRoot**要素、同様に名前付きパラメーターを提供することが指定されていませんの値を設定、 **OutputRoot**プロパティを **.\Publish\Out**します。2 番目に到達したときに**OutputRoot**要素に、条件が評価される場合は、 **true**の値が上書きされます、 **OutputRoot**プロパティの値と、**OutDir**パラメーター。

MSBuild が発生する次の要素がという名前の項目を含む、1 つの項目グループ**ProjectsToBuild**します。


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild では、この命令を処理という名前の項目リストを作成することにより**ProjectsToBuild**します。 ここでは、項目リストが 1 つの値が含まれます&#x2014;ソリューション ファイルのファイル名とパス。

この時点で、残りの要素は、ターゲットです。 プロパティと項目からターゲットを異なる方法で処理が&#x2014;は明示的に、ユーザーが指定したか、プロジェクト ファイル内で別のコンス トラクターによって呼び出される場合を除き、基本的には、ターゲットは処理されません。 いることを思い出してください開始**プロジェクト**タグが含まれています、 **DefaultTargets**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


これにより、MSBuild を呼び出す、 **FullPublish** MSBuild.exe が呼び出されるときにターゲットがない場合は、ターゲットが指定されています。 **FullPublish**ターゲットにはすべてのタスクが含まれていません。 代わりに依存関係の一覧を単純に指定します。


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


この依存関係は MSBuild を実行する順序で、 **FullPublish**をこの一覧の表示順序でターゲットを呼び出す必要が、ターゲット。

1. 呼び出す必要がありますが、**クリーン**ターゲット。
2. 呼び出す必要がありますが、 **BuildProjects**ターゲット。
3. 呼び出す必要がありますが、 **GatherPackagesForPublishing**ターゲット。
4. 呼び出す必要がありますが、 **PublishDbPackages**ターゲット。
5. 呼び出す必要がありますが、 **PublishWebPackages**ターゲット。

### <a name="the-clean-target"></a>Clean ターゲット

**クリーン**ターゲットは、出力ディレクトリとそのすべての内容を基本的に削除すると、新しいビルドの準備として。


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


ターゲットが含まれていますが、 **ItemGroup**要素。 プロパティまたは内の項目を定義するとき、**ターゲット**要素を作成している*動的*プロパティと項目。 つまり、プロパティまたは項目はターゲットが実行されるまでに処理されません。 出力ディレクトリが存在しないか、ビルドするには、ビルド プロセスが開始されるまですべてのファイルを含む、  **\_FilesToDelete**静的アイテムとして一覧表示は、実行が進行中になるまで待機する必要があります。 そのため、リストを作成するには、ターゲット内の動的なアイテムとして。

> [!NOTE]
> この場合は、ため、**クリーン**ターゲットは、最初に実行される、動的なグループを使用する実際の必要はありません。 ただし、ある時点で異なる順序でターゲットを実行するようにこの種類のシナリオでは、動的なプロパティと項目を使用することをお勧めを勧めします。  
> 使用しない項目を宣言することを回避するためにも目指します。 特定のターゲットでのみ使用される項目があれば、ビルド プロセスで、不要なオーバーヘッドを削除するターゲット内に配置することを検討してください。


項目を動的、**クリーン**ターゲットはとても簡単ですし、組み込みの使用**メッセージ**、**削除**、および**RemoveDir**へのタスクします。

1. Logger にメッセージを送信します。
2. 削除するファイルの一覧を作成します。
3. ファイルを削除します。
4. 出力ディレクトリを削除します。

### <a name="the-buildprojects-target"></a>BuildProjects ターゲット

**BuildProjects**ターゲットは基本的に、サンプル ソリューション内のすべてのプロジェクトをビルドします。


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


このターゲットは、前のトピックで詳しく説明した[プロジェクト ファイルを理解する](understanding-the-project-file.md)タスクとターゲット プロパティと項目の参照方法を説明するためにします。 この時点では、主に関心、 **MSBuild**タスク。 このタスクを使用すると、複数のプロジェクトをビルドします。 タスクが MSBuild.exe; の新しいインスタンスを作成できません。各プロジェクトをビルド、実行中の現在のインスタンスを使用します。 この例では関心のある重要な点は、展開プロパティを示します。

- **DeployOnBuild**プロパティには、各プロジェクトのビルドが完了すると、プロジェクトの設定で、デプロイの手順を実行するために MSBuild がよう指示します。
- **DeployTarget**プロパティは、プロジェクトのビルド後に起動するターゲットを識別します。 ここで、**パッケージ**ターゲットは、配置可能な web のパッケージにプロジェクトの出力をビルドします。

> [!NOTE]
> **パッケージ**ターゲットによって呼び出された Web 発行パイプライン (WPP)、MSBuild および Web デプロイ間の統合を提供します。 見てレビュー、組み込みターゲット、WPP が提供する場合、 *Microsoft.Web.Publishing.targets* %PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web フォルダーにファイル。


### <a name="the-gatherpackagesforpublishing-target"></a>GatherPackagesForPublishing ターゲット

調査する場合、 **GatherPackagesForPublishing**ターゲット、実際にすべてのタスクを含んでいないことを確認します。 代わりに、3 つの動的な項目を定義する 1 つの項目グループが含まれています。


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


これらの項目が際に作成された展開パッケージを参照してください、 **BuildProjects**ターゲットが実行されました。 でしたで定義するこれらの項目静的に、プロジェクト ファイルまで、項目が参照するファイルが存在しないため、 **BuildProjects**ターゲットを実行します。 代わりに、項目定義する必要が動的にまでは呼び出されませんターゲット内で後に、 **BuildProjects**ターゲットを実行します。

このターゲット内では、項目は使用されません&#x2014;項目と各項目の値に関連付けられているメタデータ単にこのターゲットをビルドします。 これらの要素が処理されると、 **PublishPackages**項目が 2 つの値へのパスを含む、 *ContactManager.Mvc.deploy.cmd*ファイルとパスを*ContactManager.Service.deploy.cmd*ファイル。 Web Deploy は、プロジェクトごとに web パッケージの一部としてこれらのファイルを作成し、これらは、呼び出す必要のあるファイル、パッケージをデプロイするには、移行先サーバーにします。 これらのファイルのいずれかを開くには場合、は、さまざまなビルドに固有のパラメーター値を持つ MSDeploy.exe コマンドを基本的に表示されます。

**DbPublishPackages**項目が 1 つの値へのパスを含む、 *ContactManager.Database.deploymanifest*ファイル。

> [!NOTE]
> データベース プロジェクトをビルドすると、MSBuild プロジェクト ファイルと同じスキーマを使用して、.deploymanifest ファイルが生成されます。 データベース スキーマ (.dbschema) の場所や配置前や配置後スクリプトの詳細など、データベースを配置するために必要なすべての情報が含まれています。 詳細については、次を参照してください。 [、概要のデータベースのビルドと展開](https://msdn.microsoft.com/library/aa833165.aspx)します。


デプロイ パッケージとデータベースの配置マニフェストの作成および使用方法の詳細を学習[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)と[データベース プロジェクトの配置](deploying-database-projects.md)します。

### <a name="the-publishdbpackages-target"></a>PublishDbPackages ターゲット

簡単に言うと、 **PublishDbPackages**ターゲットをデプロイする VSDBCMD ユーティリティによって呼び出された、 **ContactManager**をターゲット環境にデータベース。 多くの意思決定と微妙な差異は、データベースの配置を構成する必要があり、この点についての詳細を学習[データベース プロジェクトの配置](deploying-database-projects.md)と[の複数の環境のデータベースの配置をカスタマイズする](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). このトピックでは、このターゲットが実際に動作する方法に注目します。

最初に、開始タグが含まれることに注意してください、**出力**属性。


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


これは、例の*ターゲット バッチ処理*します。 MSBuild プロジェクト ファイルでは、バッチ処理は、コレクションを反復処理するための手法です。 値、**出力**属性、 **「% (DbPublishPackages.Identity)」** を参照、 **Identity**のメタデータのプロパティ、 **DbPublishPackages**項目のリスト。 この表記法では、**Outputs=%***(ItemList.ItemMetadataName)*、として変換されます。

- 内の項目を分割**DbPublishPackages**同じが含まれている項目のバッチに**Identity**メタデータ値。
- バッチごとに 1 回のターゲットを実行します。

> [!NOTE]
> **Identity**の 1 つ、[組み込みメタデータ値](https://msdn.microsoft.com/library/ms164313.aspx)作成時にすべての項目に割り当てられています。 値を参照して、 **Include**属性、**項目**要素&#x2014;つまり、パスと、項目のファイル名。


この場合、同じパスとファイル名では、複数の項目はならない、ため、本質的に取り組んでいますに 1 つのバッチ サイズ。 ターゲットは、データベース パッケージごとに 1 回実行されます。

同様の表記法を確認できます、  **\_Cmd**プロパティで、適切なスイッチ VSDBCMD コマンドを構築します。


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


この場合、 **%(DbPublishPackages.DatabaseConnectionString)**、 **%(DbPublishPackages.TargetDatabase)**、および **%(DbPublishPackages.FullPath)** すべてを参照してくださいメタデータの値、 **DbPublishPackages**項目コレクション。 **\_Cmd**プロパティを使って、 **Exec**タスクは、コマンドを呼び出します。


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


この表記法では、結果として、 **Exec**の一意の組み合わせに基づいてバッチを作成するタスク、 **DatabaseConnectionString**、 **TargetDatabase**、および**FullPath**メタデータの値、およびタスクは、バッチごとに 1 回実行します。 これは、例の*タスク バッチ処理*します。 ただし、ターゲット レベルのバッチ処理では、項目のコレクションを単一項目のバッチに既に分割があるため、 **Exec**タスクが 1 回とターゲットの繰り返しごとに 1 回だけ実行されます。 つまり、このタスクは、ソリューション内の各データベース パッケージに対して 1 回、VSDBCMD ユーティリティを呼び出します。

> [!NOTE]
> ターゲットとタスクのバッチ処理の詳細については、MSBuild を参照してください。[バッチ処理](https://msdn.microsoft.com/library/ms171473.aspx)、[ターゲットのバッチの項目メタデータ](https://msdn.microsoft.com/library/ms228229.aspx)、および[タスクのバッチの項目メタデータ](https://msdn.microsoft.com/library/ms171474.aspx)します。


### <a name="the-publishwebpackages-target"></a>PublishWebPackages ターゲット

この段階では、呼び出した、 **BuildProjects**ターゲットで、サンプル ソリューションでは、各プロジェクトの web 配置パッケージを生成します。 各パッケージに付属する、 *deploy.cmd*ファイルで、ターゲット環境にパッケージを展開するために必要な MSDeploy.exe コマンドが含まれていると*SetParameters.xml*ファイルを指定します、ターゲット環境の必要な詳細情報。 呼び出したも、 **GatherPackagesForPublishing**ターゲットを含む項目コレクションが生成されますが、 *deploy.cmd*ファイルに関心があります。 基本的には、 **PublishWebPackages**ターゲットは、これらの関数を実行します。

- 操作し、 *SetParameters.xml*ターゲット環境では、適切な詳細を含めるには、各パッケージのファイルを使用して、 **XmlPoke**タスク。
- 呼び出す、 *deploy.cmd*適切なスイッチを使用して、各パッケージのファイル。

同じように、 **PublishDbPackages** 、ターゲット、 **PublishWebPackages**ターゲットでターゲット バッチ処理を使用して、ターゲットは、web パッケージごとに 1 回実行されます。


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


ターゲット内で、 **Exec**タスクが実行に使用される、 *deploy.cmd* web パッケージごとにファイル。


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Web パッケージの展開の構成の詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。

## <a name="conclusion"></a>まとめ

このトピックでは、プロジェクト ファイルの分割を使用して最初から最後の連絡先のマネージャーのサンプル ソリューションのビルドと展開プロセスを制御する方法のチュートリアルが用意されています。 このアプローチを使用するを実行する複雑なエンタープライズ規模の展開、反復可能な 1 つの手順では、ファイルを環境に固有のコマンドを実行するだけでできます。

## <a name="further-reading"></a>関連項目

プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi、William Bartholomew、ISBN: 978-0-7356-4524-0。

> [!div class="step-by-step"]
> [前へ](understanding-the-project-file.md)
> [次へ](building-and-packaging-web-application-projects.md)
