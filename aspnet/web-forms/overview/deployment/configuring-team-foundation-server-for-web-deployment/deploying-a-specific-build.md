---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: 特定のビルドを展開する |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、web のパッケージとステージングまたは運用環境と同様に、新しい変換先に、特定の以前のビルドからデータベース スクリプトを展開する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 271d084b3c69016df5be28ada032973bf7fd5a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-specific-build"></a>特定のビルドを展開します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web のパッケージと、ステージング環境または運用環境のように、新しい変換先に、特定の以前のビルドからデータベース スクリプトを展開する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によってビルドおよび配置プロセスが制御されるは、2 つのプロジェクト ファイル&#x2014;いずれか各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

これまで、このチュートリアルの一連のトピックがビルド、パッケージ化、および web アプリケーションとデータベースをシングル ステップの一部として展開する方法に重点を置きますか、自動プロセス。 ただし、一般的なシナリオでにしておく、ドロップ フォルダーに含めるビルドの一覧からデプロイするリソースを選択します。 つまり、最新のビルドには、目的のビルドを配置することがありますできません。

前のトピックで説明されている継続的インテグレーション (CI) シナリオについて考えてみます[展開を作成するビルド定義をサポートしている](creating-a-build-definition-that-supports-deployment.md)です。 Team Foundation Server (TFS) 2010 のビルド定義を作成しました。 たびに、開発者が TFS にコードをチェックイン、チーム ビルドは、コードをビルド、ビルド プロセスの一部として web のパッケージとデータベースのスクリプトを作成、他の単体テストを実行およびテスト環境に、リソースを展開します。 ポリシーによっては、保有期間のビルド定義を作成するときに構成した TFS が前のビルド数を保持します。

![](deploying-a-specific-build/_static/image1.png)

ここで、検証を実行したのかと、テスト環境にビルドをこれらのいずれかに対してテスト検証とステージング環境にアプリケーションを配置する準備ができたらをします。 その間は、開発者は、新しいコードでチェックしたことがあります。 ソリューションをリビルドして、ステージング環境に展開して、最新のビルドをステージング環境に展開したくないです。 代わりに、いることを確認し、テスト サーバー上で検証した特定のビルドを展開します。

これを実現するには、Microsoft Build Engine (MSBuild) の web のパッケージと、特定のビルドが生成されるデータベース スクリプトを検索する場所を指示する必要があります。

## <a name="overriding-the-outputroot-property"></a>OutputRoot プロパティをオーバーライドします。

[サンプル ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*ファイルという名前のプロパティを宣言して**OutputRoot**です。 名前が示すように、ビルド プロセスを生成するすべてのアイテムを含むルート フォルダーです。 *Publish.proj*ファイルを確認できますを**OutputRoot**プロパティはすべての展開のリソースのルートの場所を参照します。

> [!NOTE]
> **OutputRoot**一般的に使用されるプロパティの名前を指定します。 Visual c# および Visual Basic のプロジェクト ファイルでは、すべてのビルド出力のルートの場所を格納するには、このプロパティも宣言します。


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Web パッケージを展開し、データベースを別の場所からスクリプトには、プロジェクト ファイルの場合&#x2014;などの以前の TFS ビルドの出力&#x2014;だけをオーバーライドする必要があります、 **OutputRoot**プロパティです。 チーム ビルド サーバーに関連するビルド フォルダーにプロパティの値を設定する必要があります。 値を指定する場合は、コマンドラインから MSBuild を実行していた、でした**OutputRoot**コマンドライン引数として。


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


実際には、ただしはもスキップする、**ビルド**ターゲット&#x2014;ビルド出力を使用する予定がない場合、ソリューションのビルド場所はありません。 コマンドラインから実行するターゲットを指定して、これを行う可能性があります。


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


ただし、ほとんどの場合にしておく TFS ビルド定義に、デプロイ ロジックを構築します。 これにより、ユーザーと、**ビルドをキューに**TFS サーバーに接続を Visual Studio のインストールからデプロイを開始する権限です。

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>ビルド、ビルド定義を展開する専用の作成

次の手順では、ユーザーが 1 つのコマンドを使用してステージング環境に展開をトリガーにできるビルド定義を作成する方法について説明します。

この場合、実際に何も作成するビルド定義をたくない&#x2014;する、カスタムのプロジェクト ファイルでデプロイ ロジックを実行するようにします。 *Publish.proj*ファイルにはスキップする条件ロジックが含まれています、**ビルド**ファイルがチーム ビルドで実行されている場合は対象にします。 これは、組み込みを評価することによって、 **BuildingInTeamBuild**に自動的に設定されているプロパティ**true**チーム ビルドでは、プロジェクト ファイルを実行する場合。 その結果、ビルド処理をスキップし、単に既存のビルドを配置するプロジェクト ファイルを実行できます。

**展開を手動でトリガーするビルド定義を作成するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開しを右クリックして**ビルド**、順にクリック**ビルド定義の新規**です。

    ![](deploying-a-specific-build/_static/image2.png)
2. **全般** タブで、ビルド定義の名前 (たとえば、 **DeployToStaging**) とオプションの説明。
3. **トリガー** ] タブで [**手動-チェックインのビルドをトリガーしない、新しい**です。
4. **ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。

    ![](deploying-a-specific-build/_static/image3.png)
5. **プロセス**] タブの [、**ビルド プロセス ファイル**ドロップダウン リストのままにして**DefaultTemplate.xaml**選択します。 これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。
6. **ビルド プロセス パラメーター**テーブルで、をクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。

    ![](deploying-a-specific-build/_static/image4.png)
7. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。
8. **型の項目を**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**です。
9. 使用する展開プロセスを制御、ファイルを選択してをクリックして、カスタムのプロジェクト ファイルの場所を参照**OK**です。

    ![](deploying-a-specific-build/_static/image5.png)
10. **ビルドする項目**ダイアログ ボックスで、をクリックして**OK**です。
11. **ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクションです。
12. **の MSBuild 引数**行、環境固有のプロジェクト ファイルの場所を指定して、ビルド フォルダーの場所を表すプレース ホルダーを追加します。

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > オーバーライドする必要があります、 **OutputRoot**たびにビルドをキューの値します。 これについては、次の手順で説明します。
13. **[保存]**をクリックします。

ビルドをトリガーするときに更新する必要があります、 **OutputRoot**プロパティを展開するビルドをポイントします。

**ビルド定義から特定のビルドを配置するには**

1. **チーム エクスプ ローラー**  ウィンドウでは、ビルド定義を右クリックし、をクリックして**新しいビルドをキュー**です。

    ![](deploying-a-specific-build/_static/image7.png)
2. **ビルドをキュー**ダイアログ ボックスの**パラメーター**  タブで、展開、**詳細**セクションです。
3. **の MSBuild 引数**行の値を置き換える、 **OutputRoot**ビルド フォルダーの場所を持つプロパティです。 例えば:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > ビルド フォルダーへのパスの最後に末尾のスラッシュを含めることを確認します。
4. をクリックして**キュー**です。

ビルド キューに配置し、プロジェクト ファイルは、データベース スクリプトを展開し、web パッケージ化、ビルド格納フォルダーからで指定したときに、 **OutputRoot**プロパティです。

## <a name="conclusion"></a>まとめ

このトピックでは、web のパッケージとデータベースのスクリプトと同様に、展開のリソースを公開、前の特定のビルド分割プロジェクト ファイルの配置モデルを使用する方法について説明します。 オーバーライドする方法が説明されている、 **OutputRoot**ビルド定義のプロパティと、TFS にデプロイ ロジックを組み込む方法です。

## <a name="further-reading"></a>関連項目

ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。 キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)です。

> [!div class="step-by-step"]
> [前へ](creating-a-build-definition-that-supports-deployment.md)
> [次へ](configuring-permissions-for-team-build-deployment.md)
