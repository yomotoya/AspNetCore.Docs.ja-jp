---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: 展開をサポートするビルド定義を作成する |Microsoft Docs
author: jrjlee
description: Team Foundation Server (TFS) 2010 での任意の種類のビルドを実行する場合は、チーム プロジェクト内のビルド定義を作成する必要があります。 このトピックの「des.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33ebde3074603801945c676ace64b26ca5bbf44a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826880"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>展開をサポートするビルド定義を作成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Team Foundation Server (TFS) 2010 での任意の種類のビルドを実行する場合は、チーム プロジェクト内のビルド定義を作成する必要があります。 このトピックでは、TFS で新しいビルド定義を作成する方法とチーム ビルドでビルド プロセスの一部として web 配置を制御する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルドおよび配置プロセスでは、2 つのプロジェクト ファイル&#x2014;いずれかすべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

ビルド定義は、TFS のチーム プロジェクトのビルドが実行する方法とタイミングを制御するメカニズムです。 各ビルド定義を指定します。

- Visual Studio ソリューション ファイルまたはカスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルのように、ビルドすることです。
- ビルドを実行する必要がありますを決定する条件は、手動トリガー、継続的インテグレーション (CI) と同様に配置します。 または、ゲート チェックインします。
- Web パッケージとデータベースのスクリプトなどの展開の成果物を含むチームを構築する必要がありますを送信するビルドの場所を出力します。
- 各ビルドを保持する時間数。
- ビルド プロセスのさまざまな他のパラメーター。

> [!NOTE]
> ビルド定義の詳細については、次を参照してください。[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)します。


このトピックでは、開発者が新しいコンテンツでチェックするとき、ビルドがトリガーできるため、CI を使用するビルド定義を作成する方法を説明します。 ビルドが成功すると、ビルド サービスは、ソリューションをテスト環境を配置するカスタムのプロジェクト ファイルを実行します。

ビルドをトリガーするときに、これらの操作を行う必要があります。

- 最初に、Team Build は、ソリューションをビルドする必要があります。 このプロセスの一環として、チーム ビルドの各ソリューションの web アプリケーション プロジェクトの web 展開パッケージを生成する Web 発行パイプライン (WPP) が呼び出されます。 チーム ビルドでは、ソリューションに関連付けられているすべての単体テストも実行されます。
- ソリューションのビルドが失敗した場合、チーム ビルドする必要があります何もしないさらにします。 単体テストが失敗は、ビルド エラーとして扱う必要があります。
- ソリューションのビルドが成功すると、チーム ビルドと、ソリューションの展開を制御するカスタムのプロジェクト ファイルを実行する必要があります。 このプロセスの一環として、Team Build は、インターネット インフォメーション サービス (IIS) Web 配置ツールが移行先の web サーバーにパッケージ化された web アプリケーションをインストールする (Web 配置) を呼び出し、データベースの作成を実行する VSDBCMD.exe ユーティリティが呼び出されます移行先のデータベース サーバー上のスクリプトです。

これは、プロセスを示しています。

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションには、カスタムの MSBuild プロジェクト ファイルが含まれています。 *Publish.proj*、MSBuild またはチーム ビルドから実行することができます。 」の説明に従って[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、このプロジェクト ファイルは、web のパッケージとデータベースをターゲット環境にデプロイするロジックを定義します。 ファイルには、チーム ビルド、展開タスクを実行するだけのままで実行されている場合で、ビルドとパッケージ化プロセスを省略するロジックが含まれています。 これにより、この方法で展開を自動化するときに、ソリューションが正常にビルドされることを確認する通常ためにはあり、展開プロセスを開始する前に、単体テストを渡します。

次のセクションでは、新しいビルド定義を作成してこのプロセスを実装する方法について説明します。

> [!NOTE]
> この手順&#x2014;、1 つが自動でプロセスをビルド、テスト、およびソリューションを配置します&#x2014;テスト環境へのデプロイに最も合ったする可能性があります。 ステージングおよび運用環境を既に確認され、テスト環境で検証前のビルドからコンテンツを配置する可能性の高い多くできました。 この方法は次のトピックで説明されている[特定のビルドを展開する](deploying-a-specific-build.md)します。


### <a name="who-performs-this-procedure"></a>この手順を実行しますか。

通常は、TFS 管理者は、この手順を実行します。 場合によっては、開発者のチーム リーダーは、TFS でチーム プロジェクト コレクションの責任を負う場合があります。 新しいビルド定義を作成するためには、メンバーである必要があります、**プロジェクト コレクション ビルド管理者**ソリューションを含むチーム プロジェクト コレクションにグループ化します。

## <a name="create-a-build-definition-for-ci-and-deployment"></a>CI と配置のビルド定義を作成します。

次の手順では、CI をトリガーするビルド定義を作成する方法について説明します。 ビルドが成功すると、カスタム MSBuild プロジェクト ファイルで、ロジックを使用して、ソリューションが配置されます。

**CI と配置のビルド定義を作成するには**

1. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクト ノードを展開し、右クリックして**ビルド**、順にクリックします**ビルド定義の新規**します。

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. **全般** タブで、ビルド定義の名前 (たとえば、 **DeployToTest**) とオプションの説明。
3. **トリガー**  タブで、新しいビルドをトリガーする条件を選択します。 たとえば、次のようにソリューションをビルドし、開発者が新しいコードをチェックインするたびに、テスト環境にデプロイする場合、次のように選択します。**継続的インテグレーション**します。
4. **ビルドの既定値** タブで、**ビルド出力を次の格納フォルダーにコピー**ボックスで、ドロップ フォルダーの汎用名前付け規則 (UNC) パスを入力 (たとえば、  **\\TFSBUILD\Drops**)。

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > この格納場所は、構成したリテンション期間ポリシーによっては、いくつかのビルドを格納します。 ステージング環境または運用環境に、特定のビルドからデプロイの成果物を公開する場合は、これは、それらを検索します。
5. **プロセス** タブで、**ビルド プロセス ファイル**ドロップダウン リストのままに**DefaultTemplate.xaml**選択します。 これは、すべての新しいチーム プロジェクトに追加される既定のビルド プロセス テンプレートのいずれかです。
6. **ビルド プロセス パラメーター**テーブルをクリックして、**ビルドする項目**行をクリックして、**省略記号**ボタンをクリックします。

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**します。
8. ソリューション ファイルの場所を参照し、クリックして**OK**します。

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. **ビルドする項目**ダイアログ ボックスで、をクリックして**追加**します。
10. **アイテムの種類**ドロップダウン リストで、 **MSBuild プロジェクト ファイル**します。
11. これで、展開プロセスを制御、ファイルを選択をクリックしたカスタムのプロジェクト ファイルの場所を参照**OK**します。

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **ビルドする項目** ダイアログ ボックスでは 2 つの項目が表示されるはずです。 **[OK]** をクリックします。

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. **プロセス** タブで、**ビルド プロセス パラメーター**テーブルで、展開、**詳細**セクション。
14. **MSBuild 引数**行で、MSBuild コマンドライン引数を追加する*か*項目を構築するが必要です。 連絡先マネージャー ソリューションでは、これらの引数が必要です。

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. この例では、次のように記述されています。

    1. **DeployOnBuild = true**と**DeployTarget パッケージ =** 連絡先マネージャー ソリューションをビルドするときに、引数は必須です。 これにより、web 配置パッケージ各 web アプリケーション プロジェクトのビルド後の説明に従って作成するために MSBuild[のビルドとパッケージ化 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)します。
    2. **TargetEnvPropsFile**をビルドするときは、引数が必要です、 *Publish.proj*ファイル。 」の説明に従って、このプロパティは、環境固有の構成ファイルの場所を示します[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。
16. **リテンション期間ポリシー**  タブで、必要に応じて保持する各種類のビルドの数を構成します。
17. **[保存]** をクリックします。

## <a name="queue-a-build"></a>ビルドをキューに配置する

この時点では、少なくとも 1 つの新しいビルド定義を作成しました。 ビルド プロセスを定義したビルド定義で指定したトリガーに応じて実行されます。

CI を使用するビルド定義を構成した場合は、2 つの方法で、ビルド定義をテストできます。

- 一部のコンテンツを自動ビルドをトリガーするチーム プロジェクトをチェックインします。
- ビルドを手動でキューします。

**手動でビルドをキューに**

1. **チーム エクスプ ローラー**ウィンドウでは、ビルド定義を右クリックし、順にクリックします**新しいビルドをキュー**します。

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. **ビルドをキュー**ダイアログ ボックスが、ビルド プロパティを確認し、クリックして**キュー**します。

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

進行状況とをビルドの結果を確認する&#x2014;かどうか手動または自動でトリガーされたに関係なく&#x2014;でビルド定義をダブルクリックして、**チーム エクスプ ローラー**ウィンドウ。 開き、**ビルド エクスプ ローラー**タブ。

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

ここでは、失敗したビルドをトラブルシューティングすることができます。 個々 のビルドをダブルクリックする場合は、概要情報を表示しをクリックして詳細なログ ファイル。

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

この情報を使用して、失敗したビルドのトラブルシューティングを行うし、別のビルドを試行する前に問題に対処することができます。

> [!NOTE]
> デプロイ ロジックを実行するビルドは、ビルド サーバー先の環境に必要なすべてのアクセス許可を付与が失敗する可能性があります。 詳細については、次を参照してください。[チーム ビルド展開のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)します。


## <a name="monitor-the-build-process"></a>ビルド プロセスを監視します。

TFS では、さまざまなビルド プロセスを監視するのに役立つ機能を提供します。 たとえば、TFS は、電子メールを送信またはビルドが完了したら、タスク バーの通知領域にアラートを表示できます。 詳細については、次を参照してください。[実行とビルドの監視](https://msdn.microsoft.com/library/ms181721.aspx)します。

## <a name="conclusion"></a>まとめ

このトピックでは、TFS でビルド定義を作成する方法について説明します。 ビルド定義は、開発者がチーム プロジェクトにコンテンツをチェックインするたびに、ビルド プロセスが実行されるように、CI、構成されます。 ビルド定義は、ターゲットのサーバー環境に web パッケージとデータベースのスクリプトを展開するカスタム MSBuild プロジェクト ファイルを実行します。

自動化された展開のビルド プロセスの一部として成功するためには、ターゲット web サーバーとターゲットのデータベース サーバーのビルド サービス アカウントに適切なアクセス許可を付与する必要があります。 このチュートリアルでは、最後のトピック[チーム ビルド展開のアクセス許可を構成する](configuring-permissions-for-team-build-deployment.md)、識別し、チーム ビルド サーバーからの自動展開に必要なアクセス許可を構成する方法について説明します。

## <a name="further-reading"></a>関連項目

ビルド定義を作成する方法の詳細については、次を参照してください。[基本的なビルド定義を作成する](https://msdn.microsoft.com/library/ms181716.aspx)と[ビルド プロセスの定義](https://msdn.microsoft.com/library/ms181715.aspx)します。 キュー ビルドの詳細については、次を参照してください。[ビルドをキュー](https://msdn.microsoft.com/library/ms181722.aspx)します。

> [!div class="step-by-step"]
> [前へ](configuring-a-tfs-build-server-for-web-deployment.md)
> [次へ](deploying-a-specific-build.md)
