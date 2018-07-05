---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: コマンド ファイルの作成と展開の実行 |Microsoft Docs
author: jrjlee
description: このトピックでは、シングル ステップとして Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して再展開を実行できるようにするコマンド ファイルを構築する方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830303"
---
<a name="creating-and-running-a-deployment-command-file"></a>作成と展開コマンド ファイルの実行
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、シングル ステップの再現可能なプロセスとして Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して展開を実行できるようにするコマンド ファイルを構築する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、 [Contact Manager](the-contact-manager-solution.md)ソリューション&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[ビルド プロセスを理解する](understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="process-overview"></a>プロセスの概要

このトピックでは、作成し、これらのプロジェクト ファイルを使用して、対象となる環境への反復可能な展開を実行するコマンド ファイルを実行する方法を学習します。 基本的には、単にコマンド ファイルが MSBuild コマンドを格納する必要です。

- 環境に依存しないを実行するために MSBuild に指示*Publish.proj*ファイル。
- 通知、 *Publish.proj*ファイル、環境固有のプロジェクト設定とがどこにどのファイルが含まれます。

## <a name="create-an-msbuild-command"></a>MSBuild コマンドを作成します。

」の説明に従って[ビルド プロセスを理解する](understanding-the-build-process.md)、環境固有のプロジェクト ファイル&#x2014;など*Env Dev.proj*&#x2014;環境に依存しないにインポートするのには設計されています*Publish.proj*ビルド時にファイル。 同時に、これら 2 つのファイルは、MSBuild に構築し、ソリューションをデプロイする方法を指示する命令の完全なセットを提供します。

*Publish.proj*ファイルの使用、**インポート**環境固有のプロジェクト ファイルをインポートする要素。


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


そのため、MSBuild.exe を使用して構築および連絡先マネージャー ソリューションをデプロイするときにする必要があります。

- MSBuild.exe の実行、 *Publish.proj*ファイル。
- という名前のコマンド ライン パラメーターを指定することで、環境固有のプロジェクト ファイルの場所を指定**TargetEnvPropsFile**します。

これを行うには、MSBuild コマンドのようにこのする必要があります。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


ここでは、反復可能なシングル ステップの展開に移動する簡単なステップになります。 行う必要があるは、.cmd ファイルに、MSBuild コマンドを追加することだけです。 連絡先マネージャー ソリューションでは、Publish フォルダーにという名前のファイルが含まれます。*発行 Dev.cmd*はまさにこの役割をします。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl**という名前のログ ファイルを作成する msbuild でスイッチ*msbuild.log になります*MSBuild.exe が呼び出された作業ディレクトリにします。


実行を展開または連絡先マネージャー ソリューションを再デプロイを行う必要があるだけです、*発行 Dev.cmd*ファイル。 ファイルを実行すると、MSBuild が行われます。

- ソリューション内のすべてのプロジェクトをビルドします。
- Web アプリケーション プロジェクトの配置可能な web のパッケージを生成します。
- データベース プロジェクトの .dbschema および .deploymanifest ファイルを生成します。
- Web パッケージを web サーバーに展開します。
- データベース サーバーにデータベースを配置します。

## <a name="run-the-deployment"></a>展開を実行します。

対象となる環境のコマンド ファイルを作成したら、ファイルを実行するだけで、全体の展開を完了するはずです。

**連絡先マネージャー ソリューションをテスト環境にデプロイするには**

1. 開発者のワークステーションで Windows エクスプ ローラーを開きの場所に移動し、*発行 Dev.cmd*ファイル。
2. これを実行するファイルをダブルクリックします。
3. 場合、**ファイルを開く – セキュリティの警告** ダイアログ ボックスが表示されたら、をクリックして**実行**します。
4. 構成設定とテスト サーバーが設定するかどうか、正しく、コマンド プロンプト ウィンドウが表示されます、**ビルドに成功しました**MSBuild には、プロジェクト ファイルの処理が完了したときのメッセージします。

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. この環境にソリューションを配置した初めての場合は、テスト web サーバーのコンピューター アカウントを追加する必要があります、 **db\_datawriter の各**と**db\_datareader**上の役割、 **ContactManager**データベース。 この手順で説明されて[Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)します。

    > [!NOTE]
    > データベースを作成するときに、これらのアクセス許可を割り当てる必要があるだけです。 既定では、ビルド プロセスは再作成せずデプロイごとにデータベース&#x2014;代わりに、最新のスキーマには、既存のデータベースを比較し、必要な変更のみを行います。 結果として、初めてソリューションを配置するときにこれらのデータベース ロールをマップする必要がありますのみ必要です。
6. Internet Explorer を開き、Contact Manager アプリケーションの URL に移動 (たとえば、 `http://testweb1:85/ContactManager/`)。
7. アプリケーションが期待どおりに動作することを確認し、連絡先を追加できます。

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>まとめ

ビルドとマルチ プロジェクト ソリューションを特定の送信先の環境に展開の迅速かつ簡単な方法を提供、MSBuild の手順を含むコマンド ファイルを作成します。 繰り返し、ソリューションを複数の展開先環境を配置する必要がある場合は、複数のコマンド ファイルを作成できます。 各コマンド ファイルで MSBuild コマンドが同じユニバーサル プロジェクト ファイルをビルドが別の環境に固有のプロジェクト ファイルを指定できます。 たとえば、テスト環境、開発者に発行したり、コマンド ファイルには、この MSBuild コマンドが含まれます。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


ステージング環境に発行するコマンド ファイルには、この MSBuild コマンドが含まれます。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> 独自のサーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。


プロパティをオーバーライドするか、MSBuild コマンドの他の各種のスイッチを設定して各環境のビルド プロセスをカスタマイズすることもできます。 詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)します。

> [!div class="step-by-step"]
> [前へ](deploying-database-projects.md)
> [次へ](manually-installing-web-packages.md)
