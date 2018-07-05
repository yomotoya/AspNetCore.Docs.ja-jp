---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Web パッケージを手動でインストールする |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。 トピックの構築と Web アプリケーションをパッケージ化しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d2b2e4852d01f62feef40f8b15252737327ec4ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386628"
---
<a name="manually-installing-web-packages"></a>Web パッケージを手動でインストールします。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、インターネット インフォメーション サービス (IIS) に web 配置パッケージを手動でインポートする方法について説明します。
> 
> トピック[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)説明されているパッケージ化する、IIS Web 配置ツール (Web 配置)、Microsoft Build Engine (MSBuild) と、Web 発行パイプライン (WPP) と組み合わせて使用する方法、1 つの zip ファイルに web アプリケーション プロジェクト。 このファイルには、web 配置パッケージ (または、展開パッケージだけです) と呼ばれる一般的には、IIS が web サーバーに web アプリケーションを再作成するために必要なすべてのコンテンツと構成情報が含まれています。
> 
> Web 配置パッケージを作成したら、さまざまな方法で、IIS サーバーに発行することができます。 多数のシナリオでは、MSBuild、WPP、および Web デプロイを作成し、自動またはシングル ステップのビルドと展開プロセスの一部として web パッケージをリモートでインストールの間の統合ポイントを活用するためにします。 このプロセスについては、「 [Web パッケージを展開する](deploying-web-packages.md)します。 ただし、このことは限りません。 インターネットに接続する実稼働環境に web アプリケーションをデプロイするとします。 セキュリティ上の理由から、このような運用環境では、境界ネットワーク (DMZ、非武装地帯、およびスクリーン サブネットとも呼ばれます) で、ビルド サーバーから分離したサブネット上のファイアウォールの内側にある可能性が非常に最もです。 多くの場合で運用環境は、別のドメイン、または物理的に分離されたネットワーク上になります。
> 
> これらのシナリオで、ポート、移行先サーバー上に web パッケージと IIS に手動でインポートする唯一の選択肢があります。 Web アプリケーションを公開するための非常に効果的な手法ではまだが、このアプローチは、自動化された展開のため、&#x2014;単に、web サーバーに 1 つの zip ファイルをコピーし、ウィザードを使用してインポートする手順を説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

IIS に web 配置パッケージをインポートするこれらの高度なタスクを完了する必要があります。

- MSBuild コマンドライン、チームがビルドまたは Visual Studio 2010 を使用して web 配置パッケージを作成します。
- Web パッケージを移行先の web サーバーにコピーします。
- IIS マネージャーでアプリケーション パッケージのインポート ウィザードを使用して web パッケージをインストールし、接続文字列とサービスのエンドポイントなどの変数の値を指定します。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとチュートリアルでは、このトピックでは、web パッケージ、Web デプロイ、および、WPP の背後にある概念を理解している既にことを想定しています。 詳細については、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。

> [!NOTE]
> このトピックでと組み合わせて使用最適[Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)、必要なコンポーネントをインストールし、パッケージのインポート用の IIS の web サイトを準備する方法を説明しています。


## <a name="create-a-web-deployment-package"></a>Web 配置パッケージを作成します。

最初のタスクでは、デプロイする web アプリケーション プロジェクトの web 配置パッケージを作成します。 さまざまな方法では、web パッケージを作成できます。

**アプローチ 1: Visual Studio でビルド プロセスの一環としてパッケージを作成します。**

ビルドするたびに web 配置パッケージを作成する web アプリケーション プロジェクトを構成することができます、**パッケージ化/発行 Web**プロジェクトのプロパティ ページのタブ。 このプロセスについては、「[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。

**方法 2: MSBuild でビルド プロセスの一環としてパッケージを作成します。**

MSBuild を使用して直接、カスタム MSBuild プロジェクト ファイルまたはコマンドラインから web アプリケーション プロジェクトをビルドする場合、ビルド プロセスの一部としての web 配置パッケージを作成を含めることによって、 **DeployOnBuild = true**と**DeployTarget パッケージ =** コマンド内のプロパティ。 このプロセスについては、「[ビルド プロセスを理解する](understanding-the-build-process.md)します。

**方法 3: Visual Studio で、必要に応じてパッケージを作成します。**

Visual Studio 2010 でいつでも、web アプリケーション プロジェクトの web 配置パッケージを作成できます。 これを実行する、**ソリューション エクスプ ローラー**ウィンドウでは、web アプリケーション プロジェクトを右クリックし、順にクリックします**展開パッケージのビルド**します。

![](manually-installing-web-packages/_static/image1.png)

**方法 4: コマンドラインからの要求時にパッケージを作成します。**

コマンドラインから web 配置パッケージを作成するには呼び出すことによって、**パッケージ**MSBuild を使用して、web アプリケーション プロジェクトのターゲット。 このコマンドのようになります。


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


どちらのアプローチを使用して、最終的な結果は同じです。 WPP は、web アプリケーション プロジェクトの出力フォルダー内のさまざまな関連リソースと共にの zip ファイルとして web 配置パッケージを作成します。

![](manually-installing-web-packages/_static/image2.png)

Web パッケージを手動でインポートすることを計画している、ときに、zip ファイルのみが必要です。 ターゲット web サーバーにこのファイルをコピーし、インポート プロセスを開始することができます。

## <a name="import-a-web-package-into-iis"></a>IIS に Web パッケージをインポートします。

次の手順を使用して、ローカル ファイル システムから IIS の web サイトに web 配置パッケージをインポートすることができます。 この手順を実行する前にあることを確認します。

- Web サーバーに web デプロイ パッケージをコピーします。
- アプリケーションをホストする IIS web サーバーを構成します。

Web 展開パッケージをサポートするために、IIS web サーバーの構成の詳細については、次を参照してください。 [Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。

**IIS マネージャーを使用して web 配置パッケージをインポートするには**

1. IIS マネージャーで、**接続**ウィンドウで、IIS web サイトを右クリックし、 をポイント**デプロイ**、 をクリックし、**アプリケーションのインポート**します。

    ![](manually-installing-web-packages/_static/image3.png)
2. アプリケーション パッケージのインポート ウィザードでの**パッケージを選択して** ページで、web デプロイ パッケージの場所を参照してクリックして**次へ**します。
3. **パッケージの内容を選択**ページを必要としない、クリックしてコンテンツをオフ**次**。

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > 多くの場合に、web 配置パッケージに付属するすべてのものをインポートすることがありますしません。 たとえば、関連付けられているデータベースを置き換えるへの Web 配置を許可しない可能性があります。  
    > **アクセス許可を付与**エントリは、アプリケーション プール id が web サイトのコンテンツを格納する物理フォルダーにアクセスできるようにする移行先のファイル システムのアクセス許可を設定します。 さらに、匿名認証のユーザーを Multipurpose Internet Mail Extensions (MIME) の種類のファイルを提供するアプリケーションに、フォルダーへのアクセス許可は付与読み取られます。 場合は、これらのエントリを削除し、アクセス許可を手動で構成します。
4. **アプリケーション パッケージ情報の入力** ページで、要求された情報を提供します。

    ![](manually-installing-web-packages/_static/image5.png)
5. Web パッケージを作成するときに、WPP は、アプリケーションの構成ファイルを分析し、接続文字列とサービスのエンドポイントなど、任意の変数を検出します。 この場合、次のようになります。

    1. **アプリケーション パス**は、アプリケーションをインストールする IIS のパスです。 この設定は、WPP を作成するすべての展開パッケージに共通です。
    2. **サービスのエンドポイント アドレスを ContactService**は、アプリケーションがデプロイされた WCF サービスとの通信に使用するアドレスです。 この設定は、内のエントリには、 *web.config*ファイル。
    3. 最初の**接続文字列**設定は、Web Deploy が (この場合、ASP.NET メンバーシップ データベース内) のアプリケーションに関連付けられたデータベースの配置に使用する接続文字列。 この設定の設定に対応、**パッケージ化/発行 SQL** Visual Studio でのタブ。
    4. 2 番目の**接続文字列**設定は、アプリケーションは実際に稼働している場合、データベースとの通信に使用される接続文字列。 これで接続文字列のエントリに対応して、 *web.config*ファイル。

        > [!NOTE]
        > これらのパラメーター元の場所の詳細については、次を参照してください。 [Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。
6. **[次へ]** をクリックします。
7. 初めてこの web サイトにアプリケーションをデプロイした表示しない場合をインストールする前にすべての既存のコンテンツを削除するかどうかを指定する求め。 要件に合わせて、適切なオプションを選択し、クリックして**次**します。

    ![](manually-installing-web-packages/_static/image6.png)
8. IIS は、パッケージのインストールが完了したら、クリックして**完了**します。

    ![](manually-installing-web-packages/_static/image7.png)

この時点では、IIS に web アプリケーションを正常に公開しました。

## <a name="conclusion"></a>まとめ

このトピックでは、IIS マネージャーを使用して IIS の web サイトに web 配置パッケージをインポートする方法について説明します。 Web アプリケーションの発行には、このアプローチは、セキュリティやインフラストラクチャの制約が不可能または望ましくない、リモート展開を行うときに適しています。

## <a name="further-reading"></a>関連項目

Web パッケージを手動でインポートをサポートする IIS web サーバーを構成する方法のガイダンスについては、次を参照してください。 [Web 配置発行 (オフライン展開) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。 Web パッケージの展開の一般的なガイダンスについては、次を参照してください。[チュートリアル: Web 配置パッケージ (4 の第 1 部) を使用した Web アプリケーション プロジェクトを展開](https://msdn.microsoft.com/library/dd483479.aspx)します。

> [!div class="step-by-step"]
> [前へ](creating-and-running-a-deployment-command-file.md)
