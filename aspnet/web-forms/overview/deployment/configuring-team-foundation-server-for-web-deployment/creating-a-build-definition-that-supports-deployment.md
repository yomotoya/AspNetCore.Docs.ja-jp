---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: "展開をサポートするビルド定義を作成する |Microsoft ドキュメント"
author: jrjlee
description: "どのようなビルドの Team Foundation Server (TFS) 2010 を実行する場合は、チーム プロジェクト内でビルド定義を作成する必要があります。 このトピック des しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e5610753968328e5d0f1dba4cbbfed08480fd773
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>展開をサポートするビルド定義を作成します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> どのようなビルドの Team Foundation Server (TFS) 2010 を実行する場合は、チーム プロジェクト内でビルド定義を作成する必要があります。 このトピックでは、TFS で、新しいビルド定義を作成する方法と、チーム ビルドでのビルド プロセスの一部として web 配置を制御する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)で、ビルドおよび配置プロセスが 2 つのプロジェクト ファイル & #x 2014 で制御されている; o各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含む ne です。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

ビルド定義は、TFS のチーム プロジェクトのビルドで発生する方法とタイミングを制御するメカニズムです。 各ビルド定義を指定します。

- Visual Studio ソリューション ファイルまたはカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルと同様に、ビルドすることです。
- ビルドが実行時に決定する条件は、トリガーは手動、継続的インテグレーション (CI) のように配置します。 または、ゲート チェックインします。
- チーム ビルドが送信する web のパッケージとデータベースのスクリプトと同様に展開アーティファクトを含めて、ビルド出力の場所です。
- 各ビルドを保持する時間数。
- ビルド プロセスのさまざまな他のパラメーターです。

> [!NOTE]
> ビルド定義の詳細については、次を参照してください。[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。


このトピックでは、開発者が新しいコンテンツをチェックインする際に、ビルドがトリガーされるように、構成項目を使用するビルド定義を作成する方法を示します。 ビルドが成功すると、ビルド サービスはテスト環境にソリューションを配置するカスタム プロジェクト ファイルを実行します。

ビルドをトリガーするときに、これらの操作を行う必要があります。

- 最初に、チーム ビルドは、ソリューションをビルドする必要があります。 このプロセスの一環としては、チーム ビルドすると、web アプリケーション プロジェクト、ソリューション内の各 web 展開パッケージを生成する Web 発行パイプライン (WPP) が呼び出されます。 チーム ビルドでは、ソリューションに関連付けられている他の単体テストも実行されます。
- ソリューションのビルドが失敗した場合、チームを構築する必要があります操作を行わないさらにします。 単体テストの失敗は、ビルド エラーとして処理されます。
- ソリューションのビルドが成功した場合は、チーム ビルドとソリューションの展開を制御するカスタム プロジェクト ファイルを実行する必要があります。 このプロセスの一環として、チーム ビルドと、インターネット インフォメーション サービス (IIS) Web 配置ツールが移行先の web サーバーにパッケージ化された web アプリケーションをインストールする (Web 配置) を呼び出すし、データベースの作成を実行する VSDBCMD.exe ユーティリティが実行されます。移行先のデータベース サーバーでスクリプトを作成します。

これは、プロセスを示しています。

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションには、カスタムの MSBuild プロジェクト ファイルが含まれています。 *Publish.proj*、MSBuild またはチーム ビルドから実行することができます。 」の説明に従って[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、このプロジェクト ファイルは、web のパッケージとデータベースを対象となる環境に展開するロジックを定義します。 ファイルには、チーム ビルド、配置タスクを実行するだけのままで実行されている場合に、ビルとパッケージ化プロセスを除外するロジックが含まれています。 この方法で展開を自動化するときに通常たいソリューションが正常に構築できるようにするため、展開プロセスを開始する前に、他の単体テストを渡します。

次のセクションでは、新しいビルド定義を作成することでこのプロセスを実装する方法について説明します。

> [!NOTE]
> この procedure & #x 2014; を単一の自動化されたプロセスは次のビルド、テスト、およびソリューション & #x 2014 展開以外の場合はテスト環境への展開に最も合ったする可能性があります。 ステージングと運用環境の場合よりもずっと既にことを確認して、テスト環境で検証される前のビルドからのコンテンツを展開します。 この方法は、次のトピックに記載されて[特定のビルドを配置](deploying-a-specific-build.md)です。


### <a name="who-performs-this-procedure"></a>ユーザーが、この手順を実行しますか。

通常は、TFS 管理者は、この手順を実行します。 場合によっては、開発者チーム リーダーは、TFS でチーム プロジェクト コレクションに対して責任を負う場合があります。 新しいビルド定義を作成するためには、メンバーである必要があります、**プロジェクト コレクション ビルド管理者**ソリューションを含むチーム プロジェクト コレクションにグループ化します。

## <a name="create-a-build-definition-for-ci-and-deployment"></a>CI と配置のビルド定義を作成します。

次の手順では、CI をトリガーするビルド定義を作成する方法について説明します。 ビルドが成功すると、カスタム MSBuild プロジェクト ファイルで、ロジックを使用して、ソリューションが配置します。

**CI と配置のビルド定義を作成するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開しを右クリックして**ビルド**、順にクリック**ビルド定義の新規**です。

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. **全般** タブで、ビルド定義の名前 (たとえば、 **DeployToTest**) とオプションの説明。
3. **トリガー**  タブで、新しいビルドをトリガーする条件を選択します。 たとえば、次のようにソリューションをビルドし、開発者は新しいコードをチェックインするたびに、テスト環境にデプロイする場合、次のように選択します。**継続的インテグレーション**です。
4. **ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > この格納場所には、ポリシーによっては、保有期間を構成する複数のビルドが格納されます。 ステージングまたは運用環境に、特定のビルドからの展開アイテムをパブリッシュする場合は、これは、それらを検索します。
5. **プロセス**] タブの [、**ビルド プロセス ファイル**ドロップダウン リストのままにして**DefaultTemplate.xaml**選択します。 これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。
6. **ビルド プロセス パラメーター**テーブルで、をクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。
8. ソリューション ファイルの場所を参照し、をクリックして**OK**です。

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**です。
10. **型の項目を**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**です。
11. 使用する展開プロセスを制御、ファイルを選択してをクリックして、カスタムのプロジェクト ファイルの場所を参照**OK**です。

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **ビルドする項目** ダイアログ ボックスでは 2 つの項目が表示されるはずです。 **[OK]**をクリックします。

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. **プロセス**] タブの [、**ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクションです。
14. **の MSBuild 引数**行で、MSBuild コマンドライン引数を追加する*か*アイテムを作成する必要があります。 連絡先のマネージャーのソリューション シナリオでは、これらの引数が必要です。

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. この例では、次のように記述されています。

    1. **DeployOnBuild = true**と**DeployTarget パッケージを =** Contact Manager ソリューションをビルドするときに、引数が必要です。 これにより、web 配置パッケージ各 web アプリケーション プロジェクトのビルド後の説明に従って作成するために MSBuild[パッケージ Web アプリケーション プロジェクトのビルドと](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)です。
    2. **TargetEnvPropsFile**をビルドすると、引数が必要、 *Publish.proj*ファイル。 」の説明に従って、このプロパティは、環境固有の構成ファイルの場所を示します[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。
16. **保有ポリシー**  タブで、必要に応じてを保持する各種類のビルドの数を構成します。
17. **[保存]**をクリックします。

## <a name="queue-a-build"></a>ビルドをキューに配置する

この時点では、少なくとも 1 つの新しいビルド定義を作成しました。 ビルド定義で指定したトリガーに従って定義した、ビルド プロセスが実行されます。

構成項目を使用するビルド定義を構成する場合は、2 つの方法で、ビルド定義をテストできます。

- 自動ビルドをトリガーするチーム プロジェクトにいくつかのコンテンツで確認してください。
- ビルドを手動でキューします。

**手動でビルドをキューに登録**

1. **チーム エクスプ ローラー**  ウィンドウでは、ビルド定義を右クリックし、をクリックして**新しいビルドをキュー**です。

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. **ビルドをキュー**ダイアログ ボックスでは、ビルド プロパティを確認し、をクリックして**キュー**です。

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

進行状況とビルド & #x 2014 の以外の場合は、手動または自動をするかどうか、トリガーされた & #x 2014; に関係なく、結果を確認するビルド定義をダブルクリックして、**チーム エクスプ ローラー**ウィンドウです。 これが開き、**ビルド エクスプ ローラー**タブです。

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

ここでは、ビルドの失敗をトラブルシューティングすることができます。 個々 のビルドをダブルクリックすると場合、は、概要情報を表示しをクリックして詳細なログ ファイルにできます。

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

この情報を使用して、ビルドの失敗のトラブルシューティングを行うし、別のビルドを試行する前に問題に対処することができます。

> [!NOTE]
> デプロイ ロジックを実行するビルドは、権限がビルド サーバーの展開先の環境で必要になるまでに失敗する可能性があります。 詳細については、次を参照してください。[チーム ビルドの配置用のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)です。


## <a name="monitor-the-build-process"></a>ビルド プロセスを監視します。

TFS では、ビルド プロセスの監視に役立つ機能の広範な範囲を提供します。 たとえば、TFS は、電子メールが送信またはビルドが完了したら、タスク バーの通知領域にアラートを表示します。 詳細については、次を参照してください。[実行し、ビルドの監視](https://msdn.microsoft.com/library/ms181721.aspx)です。

## <a name="conclusion"></a>まとめ

このトピックでは、TFS のビルド定義を作成する方法について説明します。 ビルド定義は、CI の開発者は、チーム プロジェクトにコンテンツがチェックインされるたびに、ビルド プロセスが実行されるように構成されます。 ビルド定義は、web のパッケージとデータベース スクリプトを対象サーバー環境に展開するカスタム MSBuild プロジェクト ファイルを実行します。

自動の展開をビルド処理の一部として成功させるためには、ターゲット web サーバーとターゲット データベースのサーバー上のビルド サービス アカウントに適切なアクセス許可を付与する必要があります。 このチュートリアルの最後のトピック[チーム ビルドの配置用のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)、識別し、チーム ビルド サーバーからの自動展開に必要なアクセス許可を構成する方法について説明します。

## <a name="further-reading"></a>関連項目

ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)です。 キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)です。

>[!div class="step-by-step"]
[前へ](configuring-a-tfs-build-server-for-web-deployment.md)
[次へ](deploying-a-specific-build.md)
