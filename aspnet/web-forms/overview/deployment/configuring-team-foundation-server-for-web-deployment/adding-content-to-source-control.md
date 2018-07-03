---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: ソース管理へのコンテンツの追加 |Microsoft Docs
author: jrjlee
description: このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。 ソリューションとプロジェクトをチーム プロジェクトに追加する方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: b4cbe16915919bcdbabcc3f9769beb15720af5eb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362996"
---
<a name="adding-content-to-source-control"></a>ソース管理へのコンテンツの追加
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。 ソリューションとプロジェクトを TFS でチーム プロジェクトに追加する方法について説明し、フレームワーク、またはアセンブリなどの外部の依存関係をソース管理に追加する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

開発者チームのすべてのメンバーは、ほとんどの場合、ソース管理にコンテンツを追加できる必要があります。 TFS でソース管理にソリューションを追加するには、これらの大まかな手順を完了する必要があります。

- チーム プロジェクトに接続します。
- サーバー上のチーム プロジェクトのフォルダー構造をローカル コンピューター上のフォルダー構造にマップします。
- ソリューションとその内容をソース コントロールに追加します。
- ソース管理には、外部依存関係を追加します。

このトピックでは、これらの手順を実行する方法を説明します。

タスクとチュートリアルでは、このトピックでは、コンテンツを管理する新しい TFS チーム プロジェクトを既に作成したことを想定しています。 新しいチーム プロジェクトを作成する方法の詳細については、次を参照してください。 [TFS でチーム プロジェクトを作成する](creating-a-team-project-in-tfs.md)します。

### <a name="who-performs-these-procedures"></a>これらの手順を実行しますか。

ほとんどの場合、開発者チームのすべてのメンバーを追加し、特定のチーム プロジェクト内のコンテンツを変更する必要がありますできます。

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>チーム プロジェクトに接続し、フォルダー マッピングの作成

任意のコンテンツをソース管理に追加する前に、チーム プロジェクトに接続し、ローカル コンピューターで、サーバー上のフォルダー構造とファイル システム間のマッピングを作成する必要があります。

**チーム プロジェクトに接続し、ローカル パスをマップするには**

1. 開発者のワークステーションでは、Visual Studio 2010 を開きます。
2. Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**します。

    > [!NOTE]
    > TFS サーバーへの接続を既に構成した場合は、手順 3. ~ 6. を省略できます。
3. **チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**します。
4. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**します。
5. **Team Foundation Server の追加** ダイアログ ボックスでは、TFS インスタンスの詳細を入力し、順にクリックします**OK**します。

    ![](adding-content-to-source-control/_static/image1.png)
6. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**します。
7. **チーム プロジェクトへの接続**ダイアログ ボックスで、接続するチームを選択する TFS インスタンスを選択はプロジェクト コレクション、選択、追加するチーム プロジェクトと順にクリックします**Connect**します。

    ![](adding-content-to-source-control/_static/image2.png)
8. **チーム エクスプ ローラー**ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**します。

    ![](adding-content-to-source-control/_static/image3.png)
9. **ソース管理エクスプ ローラー** ] タブで [**マップされていない**します。

    ![](adding-content-to-source-control/_static/image4.png)
10. **マップ** ダイアログ ボックスで、**ローカル フォルダー**クリックして、チーム プロジェクトのルート フォルダーとして機能するローカル フォルダーのボックスを参照 (または作成)**マップ**します。

    ![](adding-content-to-source-control/_static/image5.png)
11. ソース ファイルをダウンロードするメッセージが表示されたら、クリックして**はい**します。

    ![](adding-content-to-source-control/_static/image6.png)

この時点では、開発者のワークステーション上のローカル フォルダーをチーム プロジェクトのサーバー側のフォルダーにマップします。 既存の内容は、ローカル フォルダー構造に、チーム プロジェクトからもダウンロードしました。 ソース管理に独自のコンテンツを追加することができますようになりました。

## <a name="add-projects-and-solutions-to-source-control"></a>プロジェクトとソリューションをソース管理に追加します。

ソース管理にプロジェクトとソリューションを追加するには、まず、ローカル コンピューターに、チーム プロジェクトのマップされたフォルダーに移動する必要があります。 サーバーと、追加機能を同期するコンテンツで確認できます。

**ソース管理にプロジェクトを追加するには**

1. 開発者のワークステーションで、プロジェクトやソリューションをチーム プロジェクトのマップされたフォルダー構造内の適切な場所に移動します。

    > [!NOTE]
    > 多くの組織では、プロジェクトおよびソリューションはソース管理して整理する方法の推奨されるアプローチがあります。 フォルダーを構造にする方法のガイダンスについては、次を参照してください。 [How To: 構造体のソースの管理フォルダーで Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx)します。
2. Visual Studio 2010 でのソリューションを開きます。
3. **ソリューション エクスプ ローラー**ウィンドウで、ソリューションを右クリックし、順にクリックします**ソリューションをソース管理に追加**します。

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > 場合によっては、組織が TFS では、構造のコンテンツを好きな方法によっては、ソース コードの編成方法きめの細かい制御を提供するには、個別にソース管理にプロジェクトを追加する必要があります。
4. いることを確認、**ソース管理エクスプ ローラー**  タブには、チーム プロジェクトのサーバーのフォルダー構造内に追加したコンテンツが表示されます。

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **ソース管理エクスプ ローラー**  タブには、ローカル ファイル システム上のマップされたフォルダーにソリューションを追加するためにそれ以上入力を求めるでコンテンツが表示されます。 マップされていない場所に、ソリューションであった場合は、TFS とローカル ファイル システムの両方でフォルダーの場所を指定する求めるは。
5. **ソース管理エクスプ ローラー**  タブで、**フォルダー**ウィンドウで、チーム プロジェクトを右クリックして (たとえば、 **ContactManager**)、順にクリックします**チェックイン保留中の変更**します。
6. **チェックイン – ソース ファイル**ダイアログ ボックスで、コメントを入力し、順にクリックします**チェックイン**します。

    ![](adding-content-to-source-control/_static/image9.png)

この時点で、ソリューションを TFS でソース管理に追加しました。

## <a name="add-external-dependencies-to-source-control"></a>ソース管理への外部の依存関係を追加します。

プロジェクトまたはソリューションをソース管理に追加すると、ファイルやプロジェクトまたはソリューション内のフォルダーも追加されます。 ただし、多くの場合に、プロジェクトおよびソリューションも使用して正常に機能する、ローカル アセンブリのように、外部の依存関係します。 コードは正常にビルドをソース管理により、チーム ビルドその開発者チームの他のメンバーにこのようなすべてのリソースを追加する必要があります。

たとえば、連絡先マネージャーのサンプル ソリューションのフォルダー構造には、パッケージをという名前のフォルダーが含まれます。 ADO.NET の Entity Framework 4.1 のアセンブリとサポートするさまざまなリソースが含まれます。 パッケージ フォルダーは、連絡先マネージャー ソリューションの一部ではありませんが、しなくても、ソリューションが正常にビルドされません。 ソリューションをビルドするチームがビルドを有効にするには、パッケージ フォルダーをソース管理に追加する必要があります。

> [!NOTE]
> パッケージ フォルダーを含めることは、Visual Studio 2010 用の NuGet 拡張機能を使用して、ソリューションに、Entity Framework、または同様のリソースを追加するときの動作の一般的な例です。


**ソース管理にプロジェクト以外のコンテンツを追加するには**

1. (たとえば、パッケージ フォルダーなど) を追加する項目が、ローカル ファイル システム上のマップされたフォルダー内の適切な場所であることを確認します。
2. Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**します。

    ![](adding-content-to-source-control/_static/image10.png)
3. **ソース管理エクスプ ローラー**  タブで、**フォルダー**ウィンドウで、追加する項目を格納またはアイテムをフォルダーを選択します。
4. をクリックして、**フォルダーに項目の追加**ボタンをクリックします。

    ![](adding-content-to-source-control/_static/image11.png)
5. **ソース管理に追加** ダイアログ ボックスで、フォルダーまたは追加するをクリックし項目を選択**次**します。

    ![](adding-content-to-source-control/_static/image12.png)
6. **項目を除外** タブで、除外されている自動的に (たとえば、アセンブリ) をクリックして必要な項目を選択**項目の追加**します。

    ![](adding-content-to-source-control/_static/image13.png)
7. **追加する項目** タブで、追加するすべてのファイルが一覧表示されます。 順にクリックしますであることを確認**完了**します。

    ![](adding-content-to-source-control/_static/image14.png)
8. **ソース管理エクスプ ローラー**ウィンドウで、をクリックして、**チェックイン**ボタンをクリックします。

    ![](adding-content-to-source-control/_static/image15.png)
9. **チェックイン – ソース ファイル**ダイアログ ボックスで、コメントを入力し、順にクリックします**チェックイン**します。

この時点では、ソース管理にソリューションの外部の依存関係を追加があります。

## <a name="conclusion"></a>まとめ

このトピックでは、チーム プロジェクトに接続し、フォルダー構造をマップし、ソース管理にコンテンツを追加する方法について説明します。 ソース管理下にある項目を操作する方法の詳細については、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)します。

次のトピックでは、 [Web 配置の TFS ビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)、構築して、ソリューションをデプロイする TFS チーム ビルド サーバーを準備する方法について説明します。

## <a name="further-reading"></a>関連項目

TFS でソース管理の操作方法の詳細については、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)します。

> [!div class="step-by-step"]
> [前へ](creating-a-team-project-in-tfs.md)
> [次へ](configuring-a-tfs-build-server-for-web-deployment.md)
