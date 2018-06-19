---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Web パッケージを手動でインストールする |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。 トピックのビルドとパッケージ Web Applicati しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890217"
---
<a name="manually-installing-web-packages"></a>Web パッケージを手動でインストールします。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。
> 
> トピック[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)説明されている IIS Web 配置ツール (Web 配置)、Microsoft Build Engine (MSBuild) と、Web 発行パイプライン (WPP) と組み合わせての使用方法をパッケージ化する、1 つの zip ファイルに web アプリケーション プロジェクト。 Web 配置パッケージ (または展開パッケージだけ) と呼ばれます、このファイルには、IIS が web サーバーに web アプリケーションを再作成するために必要なすべてのコンテンツと構成情報が含まれています。
> 
> Web 配置パッケージを作成した後は、さまざまな方法で、IIS サーバーにパブリッシュできます。 多数のシナリオでは、MSBuild、WPP、および Web デプロイを作成し、自動または 1 ステップのビルドおよび配置プロセスの一部として web パッケージをリモートでインストールの間の統合ポイントを活用するためにします。 このプロセスについては、「 [Web パッケージを展開する](deploying-web-packages.md)です。 ただし、これは常に可能です。 インターネットに接続された運用環境に web アプリケーションを展開するとします。 セキュリティ上の理由から、このような実稼働環境では、境界ネットワーク (DMZ、非武装地帯およびスクリーン サブネットとも呼ばれます) で、ビルド サーバーから分離したサブネット上のファイアウォールの内側にある可能性が非常に最もです。 多くの場合では、運用環境は別のドメインにまたは物理的に分離ネットワーク上になります。
> 
> これらのシナリオで、移行先サーバー上に web パッケージのポートを IIS に手動でインポートする唯一の選択肢があります。 この方法では、自動展開が除外はまだ web アプリケーションを公開するための非常に効果的な手法&#x2014;する、web サーバーに 1 つの zip ファイルをコピーし、インポート プロセスを案内するウィザードを使用します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

IIS に web 配置パッケージをインポートするこれらの高度なタスクを完了する必要があります。

- MSBuild コマンドライン、チーム ビルド、または Visual Studio 2010 を使用して、web 配置パッケージを作成します。
- Web パッケージを移行先の web サーバーにコピーします。
- アプリケーション パッケージのインポート ウィザードで IIS マネージャーを使用して web パッケージをインストールし、接続文字列とサービスのエンドポイントと同様に変数の値を指定します。

このトピックでは、これらの手順を実行する方法を示します。 タスクとこのトピックの「チュートリアルは、web パッケージ、Web デプロイおよび WPP 考え方に慣れている既にことを想定しています。 詳細については、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。

> [!NOTE]
> このトピックの内容と組み合わせて使用最適[用 Web 配置発行 (オフライン展開) Web サーバーの構成](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)、必須コンポーネントをインストールおよびパッケージのインポート用の IIS の web サイトを準備する方法について説明しています。


## <a name="create-a-web-deployment-package"></a>Web 配置パッケージを作成します。

最初のタスクでは、web アプリケーション プロジェクトを展開するための web 配置パッケージを作成します。 さまざまな方法では、web のパッケージを作成できます。

**アプローチ 1: Visual Studio でのビルド プロセスの一環として、パッケージを作成します。**

各ビルド後に web 配置パッケージを作成する web アプリケーション プロジェクトを構成することができます、**パッケージ化/発行 Web**プロジェクトのプロパティ ページのタブです。 このプロセスについては、「[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。

**アプローチ 2: MSBuild を使用して、ビルド プロセスの一部としてパッケージを作成します。**

MSBuild を使用して直接、カスタム MSBuild プロジェクト ファイルまたはコマンドラインから、web アプリケーション プロジェクトをビルドする場合、ビルド プロセスの一部としての web 配置パッケージする作成を含めることによって、 **DeployOnBuildはtrueを=** と**DeployTarget パッケージを =** コマンド内のプロパティです。 このプロセスについては、「[ビルド プロセスの理解](understanding-the-build-process.md)です。

**アプローチ 3: Visual Studio でのオンデマンドのパッケージを作成します。**

Web アプリケーション プロジェクトの web 配置パッケージは、Visual Studio 2010 でいつでも作成できます。 これを行うで、**ソリューション エクスプ ローラー**  ウィンドウでは、web アプリケーション プロジェクトを右クリックし、をクリックして**展開パッケージのビルド**です。

![](manually-installing-web-packages/_static/image1.png)

**方法 4: コマンドラインからの要求時にパッケージを作成します。**

コマンドラインから web 配置パッケージを作成するには呼び出すことによって、**パッケージ**MSBuild を使用して、web アプリケーション プロジェクトのターゲットです。 このコマンドは、このようになります。


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


どちらアプローチを使用して、最終的な結果は同じです。 WPP は、web アプリケーション プロジェクトの出力フォルダー内のさまざまなサポート リソースと共にの zip ファイルとして web 配置パッケージを作成します。

![](manually-installing-web-packages/_static/image2.png)

Web パッケージを手動でインポートを計画するときは、zip ファイルのみが必要です。 ターゲット web サーバーにこのファイルをコピーし、インポート プロセスを開始することができます。

## <a name="import-a-web-package-into-iis"></a>IIS に Web パッケージをインポートします。

次の手順を使用して、ローカル ファイル システムから IIS の web サイトに web 配置パッケージをインポートすることができます。 この手順を実行する前にあることを確認します。

- Web サーバーに web 配置パッケージをコピーします。
- アプリケーションをホストする IIS web サーバーを構成します。

Web 配置パッケージをサポートするために IIS web サーバーを構成する方法については、次を参照してください。[用 Web 配置発行 (オフライン展開) Web サーバーの構成](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)です。

**IIS マネージャーを使用して web 展開パッケージをインポートするには**

1. IIS マネージャーで、**接続**ウィンドウで、IIS web サイトを右クリックし、順にポイント**展開**、クリックして**アプリケーションのインポート**です。

    ![](manually-installing-web-packages/_static/image3.png)
2. アプリケーション パッケージのインポート ウィザードでの**パッケージを選択して** ページで、web 配置パッケージの場所を参照してクリックして**次へ**です。
3. **パッケージの内容の選択**ページで、をクリックして、必要しないコンテンツをオフ**次**です。

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > 多くの場合、web 配置パッケージに付属するすべてのアイテムをインポートすることがありますしません。 たとえば、Web が関連付けられているデータベースを置換する展開を許可することがあります避けたいです。  
    > **アクセス許可を与える**エントリは、アプリケーション プール id が web サイトのコンテンツを格納する物理フォルダーにアクセスできるようにする移行先のファイル システムのアクセス許可を設定します。 さらに、匿名認証ユーザー read 権限がフォルダーにアプリケーションを Multipurpose Internet Mail Extensions (MIME) の種類のファイルを提供します。 必要に応じて、これらのエントリを削除するでき、アクセス許可を手動で構成できます。
4. **アプリケーション パッケージ情報の入力** ページで、要求された情報を提供します。

    ![](manually-installing-web-packages/_static/image5.png)
5. Web パッケージを作成するときに、WPP は、アプリケーションの構成ファイルを分析し、接続文字列とサービスのエンドポイントと同様に、任意の変数を検出します。 この場合、次のようになります。

    1. **アプリケーション パス**は、アプリケーションをインストールする IIS のパスです。 この設定は、WPP を作成するすべての展開パッケージに共通です。
    2. **ContactService サービス エンドポイント アドレス**アプリケーションが展開されている WCF サービスとの通信に使用するアドレスです。 この設定内のエントリに対応、 *web.config*ファイル。
    3. 最初の**接続文字列**設定は、Web Deploy が、アプリケーション (この場合は ASP.NET メンバーシップ データベース) に関連付けられているデータベースの配置に使用する接続文字列。 この設定の設定に対応、**パッケージ化/発行 SQL** Visual Studio でのタブです。
    4. 2 番目**接続文字列**設定は、アプリケーションは立ち上がっており実行中であるときに、データベースと通信するために実際に使用される接続文字列。 これで、接続文字列のエントリに対応して、 *web.config*ファイル。

        > [!NOTE]
        > どこから取得されるこれらのパラメーターの詳細については、次を参照してください。 [Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)です。
6. **[次へ]** をクリックします。
7. これはこの web サイトにアプリケーションを配置した最初の時間ではない場合、インストールする前にすべての既存コンテンツを削除するかどうかを指定する求められます。 要件を適切なオプションを選択し、クリックして**次**です。

    ![](manually-installing-web-packages/_static/image6.png)
8. IIS は、パッケージのインストールが完了したらをクリックして**完了**です。

    ![](manually-installing-web-packages/_static/image7.png)

この時点では、IIS に web アプリケーションを正常に発行しました。

## <a name="conclusion"></a>まとめ

このトピックでは、IIS マネージャーを使用して IIS の web サイトに web 配置パッケージをインポートする方法について説明します。 Web アプリケーションの発行には、この方法は、セキュリティまたはインフラストラクチャの制約が不可能または望ましくないのリモート展開を行うときに適しています。

## <a name="further-reading"></a>関連項目

Web パッケージを手動でインポートをサポートする IIS web サーバーを構成する方法のガイダンスについては、次を参照してください。[用 Web 配置発行 (オフライン展開) Web サーバーの構成](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)です。 Web パッケージの展開の一般的なガイダンスについては、次を参照してください。[チュートリアル: Web 配置パッケージ (4 のパート 1) を使用して Web アプリケーション プロジェクトを配置](https://msdn.microsoft.com/library/dd483479.aspx)です。

> [!div class="step-by-step"]
> [前へ](creating-and-running-a-deployment-command-file.md)
