---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "コマンド ファイルの作成と展開を実行 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、ファイルを構築するコマンドの 1 つの手順として、Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して再展開を実行するために使用する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>作成と展開コマンド ファイルの実行
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、1 ステップの再現可能なプロセスとして Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して展開を実行するために使用するコマンド ファイルを作成する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルのシリーズを使用してサンプル ソリューション & #x 2014;、[連絡先のマネージャー](the-contact-manager-solution.md)複雑さ、ASP.NET MVC 3 アプリケーション、Windows などの現実的なレベルで web アプリケーションを表すためには、ソリューション & #x 2014;Communication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[ビルド プロセスの理解](understanding-the-build-process.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="process-overview"></a>プロセスの概要

このトピックでは、作成し、これらのプロジェクト ファイルを使用して、対象となる環境への反復可能な展開を実行するコマンド ファイルを実行する方法を学習します。 基本的には、コマンド ファイル単純にする必要があります、MSBuild コマンドが含まれています。

- 環境にもとらわれないを実行する MSBuild に指示*Publish.proj*ファイル。
- 通知、 *Publish.proj*ファイルのどのファイルには、環境固有のプロジェクト設定とそれを検索する場所が含まれています。

## <a name="create-an-msbuild-command"></a>MSBuild のコマンドを作成します。

」の説明に従って[ビルド プロセスの理解](understanding-the-build-process.md)、環境固有のプロジェクト ファイル & #x 2014; など、 *Env Dev.proj*& #x 2014; にインポートするよう設計されています、環境にもとらわれない*Publish.proj*ビルド時にファイル。 さらに、これら 2 つのファイルには、MSBuild のビルドし、ソリューションを展開する方法を指示する命令の完全なセットが提供されます。

*Publish.proj*ファイルの使用、**インポート**環境固有のプロジェクト ファイルをインポートする要素。


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


そのため、MSBuild.exe を使用してビルドして、連絡先のマネージャー ソリューションを配置するときにする必要があります。

- MSBuild.exe を実行、 *Publish.proj*ファイル。
- という名前のコマンド ライン パラメーターを指定することによって、環境固有のプロジェクト ファイルの場所の指定**TargetEnvPropsFile**です。

これを行うには、MSBuild コマンドのようにこの必要があります。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


ここでは、これは、反復可能な単一ステップ デプロイメントに移動する簡単な手順です。 必要な操作すべては、.cmd ファイルを MSBuild コマンドを追加します。 ソリューションでは、連絡先のマネージャー、発行フォルダーには、という名前のファイルが含まれる*発行 Dev.cmd*正確にこれを行うことです。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl**という名前のログ ファイルを作成するスイッチが msbuild *msbuild.log になります*MSBuild.exe が呼び出された作業ディレクトリにします。


連絡先のマネージャー ソリューションを再配置を配置するには、行う必要がある実行、*発行 Dev.cmd*ファイル。 MSBuild は、ファイルを実行するときに行います。

- ソリューション内のすべてのプロジェクトをビルドします。
- Web アプリケーション プロジェクトの配置可能な web パッケージを生成します。
- データベース プロジェクトの .dbschema および .deploymanifest ファイルを生成します。
- Web サーバーに web パッケージを展開します。
- データベース サーバーにデータベースを展開します。

## <a name="run-the-deployment"></a>展開を実行します。

ターゲット環境には、コマンド ファイルを作成したら、単にファイルを実行して、全体の展開を完了することができます。

**連絡先のマネージャー ソリューション、テスト環境を配置するには**

1. 開発者のワークステーションで Windows エクスプ ローラーを開きの場所を参照し、*発行 Dev.cmd*ファイル。
2. これを実行するファイルをダブルクリックします。
3. 場合、**ファイルを開く-セキュリティ警告** ダイアログ ボックスが表示されたら、をクリックして**実行**です。
4. かどうか、構成設定およびテスト サーバーが設定されて正しく、コマンド プロンプト ウィンドウが表示されます、**ビルドに成功しました**メッセージの MSBuild がプロジェクト ファイルの処理を完了します。

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. この環境にソリューションを配置した初めての場合は、テスト web server マシン アカウントを追加する必要があります、 **db\_datawriter**と**db\_datareader**上の役割、 **ContactManager**データベース。 この手順で説明されて[Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)です。

    > [!NOTE]
    > データベースを作成するときに、これらのアクセス許可を割り当てる必要があるだけです。 既定では、ビルド処理はすべての配置 & #x 2014 データベース再作成されません。 代わりを最新のスキーマに既存のデータベースを比較し、必要な変更のみを行います。 結果として、初めてにソリューションを配置するときにこれらのデータベース ロールをマップする必要がありますのみ必要。
6. Internet Explorer を開き、連絡先のマネージャー アプリケーションの URL に移動 (たとえば、 `http://testweb1:85/ContactManager/`)。
7. アプリケーションが期待どおりに動作することを確認し、連絡先を追加することができました。

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>まとめ

MSBuild 指示を含むコマンド ファイルを作成するビルドと特定の送信先の環境にマルチ プロジェクト ソリューションを展開の迅速かつ簡単な方法を提供しています。 繰り返し、ソリューションを複数の展開先環境を配置する必要がある場合は、複数のコマンド ファイルを作成できます。 各コマンド ファイルで、MSBuild のコマンドは、同じユニバーサル プロジェクト ファイルのビルド、けれども、別の環境に固有のプロジェクト ファイルを指定できます。 たとえば、またはテスト環境で、開発者に公開するコマンド ファイルには、この MSBuild コマンドが含まれます。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


コマンド ファイル ステージング環境に公開するにはには、この MSBuild コマンドが含まれます。


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。


プロパティをオーバーライドするか、MSBuild コマンドで他の各種のスイッチを設定して各環境のビルド プロセスをカスタマイズすることもできます。 詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。

>[!div class="step-by-step"]
[前へ](deploying-database-projects.md)
[次へ](manually-installing-web-packages.md)
