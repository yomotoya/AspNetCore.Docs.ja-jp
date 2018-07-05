---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: 特定のビルドを展開する |Microsoft Docs
author: jrjlee
description: このトピックでは、web パッケージと新しい変換先のステージング環境または運用のためのフローチャートのように、特定の以前のビルドからのデータベース スクリプトをデプロイする方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388225"
---
<a name="deploying-a-specific-build"></a>特定のビルドを展開します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、web のパッケージと新しい変換先のステージング環境または運用環境と同じように、特定の以前のビルドからのデータベース スクリプトをデプロイする方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルドおよび配置プロセスでは、2 つのプロジェクト ファイル&#x2014;いずれかすべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

これまで、この一連のチュートリアルのトピックがビルド、パッケージ化、および web アプリケーションとデータベースをシングル ステップの一部として展開する方法に重点を置いてまたは、プロセスを自動化します。 ただし、一般的なシナリオでビルド ドロップ フォルダーの一覧から、デプロイするリソースを選択するします。 つまり、最新のビルドには、ビルドをデプロイすることがありますできません。

前のトピックで説明されている継続的インテグレーション (CI) シナリオを考えます。[配置を作成する、ビルド定義をサポートしています](creating-a-build-definition-that-supports-deployment.md)します。 Team Foundation Server (TFS) 2010 では、ビルド定義を作成しました。 たびに、開発者は、TFS にコードをチェック、チーム ビルドは、コードをビルド、ビルド プロセスの一部として web パッケージとデータベースのスクリプトを作成、単体テストを実行およびテスト環境にリソースをデプロイします。 ビルド定義を作成したときに構成したリテンション期間ポリシーによっては、前のビルド数を TFS が保持されます。

![](deploying-a-specific-build/_static/image1.png)

ここと検証を実行したのかと検証に対してこれらのいずれかのテストをビルド、テスト環境内でステージング環境にアプリケーションを配置する準備ができました。 それまでは、開発者は、新しいコードでチェックがある可能性があります。 ソリューションをリビルドし、ステージング環境に配置しないし、最新のビルドをステージング環境にデプロイします。 代わりに、いることを確認し、テスト サーバーで検証した特定のビルドをデプロイします。

これを実現するには、Microsoft Build Engine (MSBuild) に web パッケージと特定のビルドが生成されたデータベースのスクリプトを検索する場所を指示する必要があります。

## <a name="overriding-the-outputroot-property"></a>OutputRoot プロパティをオーバーライドします。

[サンプル ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)、 *Publish.proj*ファイルという名前のプロパティを宣言する**OutputRoot**します。 名前からわかるように、これは、ビルド プロセスによって生成されるすべてのものを含むルート フォルダー。 *Publish.proj*ファイルを確認できますが、 **OutputRoot**プロパティはすべての展開リソースのルートの場所を参照します。

> [!NOTE]
> **OutputRoot**一般的に使用されるプロパティの名前を指定します。 Visual c# および Visual Basic のプロジェクト ファイルでは、すべてのビルド出力のルートの場所を格納するには、このプロパティも宣言します。


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Web パッケージを展開し、データベースを別の場所からスクリプトをプロジェクト ファイルの場合&#x2014;などの以前の TFS ビルドの出力&#x2014;だけをオーバーライドする必要があります、 **OutputRoot**プロパティ。 チーム ビルド サーバーで、関連するビルド フォルダーにプロパティ値を設定する必要があります。 コマンドラインから MSBuild を実行する場合は、値を指定できます**OutputRoot**コマンドライン引数として。


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


実際には、ただしはもスキップする、**ビルド**ターゲット&#x2014;ビルド出力を使用する予定がない場合、ソリューションの構築に意味がありません。 これをコマンドラインから実行するターゲットを指定して実行できます。


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


ただし、ほとんどの場合、TFS ビルド定義に、デプロイ ロジックを構築するします。 これにより、ユーザーに、**ビルドをキューに**TFS サーバーに接続を持つ任意の Visual Studio 環境からデプロイを開始するためのアクセス許可。

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>ビルドの特定を展開するビルド定義を作成します。

次の手順では、ユーザーを 1 つのコマンドを使用してステージング環境へのデプロイをトリガーできるようにビルド定義を作成する方法について説明します。

この場合は、実際に何も作成するビルド定義がしない&#x2014;、カスタムのプロジェクト ファイルで、デプロイ ロジックを実行するようにするだけです。 *Publish.proj*ファイルにはスキップする条件付きロジックが含まれています、**ビルド**ターゲット ファイルは、チーム ビルドで実行している場合。 これは、組み込みを評価することによって、 **BuildingInTeamBuild**に自動的に設定されているプロパティ、 **true**チーム ビルドでプロジェクト ファイルを実行する場合。 その結果、ビルド プロセスをスキップし、単に既存のビルドを配置するプロジェクト ファイルを実行できます。

**展開を手動でトリガーするビルド定義を作成するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開し、右クリックして**ビルド**、順にクリックします**ビルド定義の新規**します。

    ![](deploying-a-specific-build/_static/image2.png)
2. **全般** タブで、ビルド定義の名前 (たとえば、 **DeployToStaging**) とオプションの説明。
3. **トリガー** ] タブで [**手動-チェックイン ビルドをトリガーしない、新しい**します。
4. **ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。

    ![](deploying-a-specific-build/_static/image3.png)
5. **プロセス** タブで、**ビルド プロセス ファイル**ドロップダウン リストのままに**DefaultTemplate.xaml**選択します。 これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。
6. **ビルド プロセス パラメーター**テーブルをクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。

    ![](deploying-a-specific-build/_static/image4.png)
7. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**します。
8. **アイテムの種類**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**します。
9. これで、展開プロセスを制御、ファイルを選択をクリックしたカスタムのプロジェクト ファイルの場所を参照**OK**します。

    ![](deploying-a-specific-build/_static/image5.png)
10. **ビルドする項目**ダイアログ ボックスで、をクリックして**OK**します。
11. **ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクション。
12. **MSBuild 引数**行で、環境固有のプロジェクト ファイルの場所を指定してビルド フォルダーの場所のプレース ホルダーを追加します。

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > オーバーライドする必要があります、 **OutputRoot**値のたびに、ビルドがキューに配置します。 これについては、次の手順で説明します。
13. **[保存]** をクリックします。

ビルドをトリガーするときに更新する必要があります、 **OutputRoot**プロパティを展開するビルドをポイントします。

**ビルド定義から特定のビルドをデプロイするには**

1. **チーム エクスプ ローラー**ウィンドウでは、ビルド定義を右クリックし、順にクリックします**新しいビルドをキュー**します。

    ![](deploying-a-specific-build/_static/image7.png)
2. **ビルドをキュー**  ダイアログ ボックスの 、**パラメーター**  タブで、展開、**詳細**セクション。
3. **MSBuild 引数**行の値を置き換える、 **OutputRoot**プロパティ、ビルド フォルダーの場所を使用します。 例えば:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > 必ず、ビルド フォルダーへのパスの最後に末尾のスラッシュが含まれます。
4. クリックして**キュー**します。

ビルドをキューし、プロジェクト ファイルは、データベース スクリプトを展開して、web がで指定したビルドの格納フォルダーからパッケージ、 **OutputRoot**プロパティ。

## <a name="conclusion"></a>まとめ

このトピックでは、web パッケージとデータベースのスクリプトのように、展開のリソースを公開、前の特定のビルド分割プロジェクト ファイルのデプロイ モデルを使用する方法について説明します。 オーバーライドする方法について説明し、 **OutputRoot**ビルド定義のプロパティと、TFS、デプロイ ロジックを組み込む方法。

## <a name="further-reading"></a>関連項目

ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)します。 キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)します。

> [!div class="step-by-step"]
> [前へ](creating-a-build-definition-that-supports-deployment.md)
> [次へ](configuring-permissions-for-team-build-deployment.md)
