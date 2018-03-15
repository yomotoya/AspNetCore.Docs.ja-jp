---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "ソース管理へのコンテンツの追加 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。 ソリューションとプロジェクトをチーム プロジェクトに追加する方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="adding-content-to-source-control"></a>ソース管理へのコンテンツの追加
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。 ソリューションとプロジェクトを TFS では、チーム プロジェクトに追加する方法を説明し、フレームワーク、またはアセンブリのような外部の依存関係をソース管理に追加する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

## <a name="task-overview"></a>タスクの概要

ほとんどの場合、開発者チームのすべてのメンバーをソース管理にコンテンツを追加することにする必要があります。 TFS でのソース管理にソリューションを追加するには、これらの大まかな手順を完了する必要があります。

- チーム プロジェクトに接続します。
- サーバー上のチーム プロジェクトのフォルダー構造をローカル コンピューター上のフォルダー構造にマップします。
- ソース管理にソリューションとその内容を追加します。
- ソース管理には、外部の依存関係を追加します。

このトピックでは、これらの手順を実行する方法を示します。

タスクとここでチュートリアルは、コンテンツを管理する新しい TFS チーム プロジェクトを既に作成したと仮定します。 新しいチーム プロジェクトを作成する方法の詳細については、次を参照してください。 [TFS でチーム プロジェクトの作成](creating-a-team-project-in-tfs.md)です。

### <a name="who-performs-these-procedures"></a>ユーザーには、これらの手順を実行しますか。

ほとんどの場合、開発者チームのすべてのメンバーを追加および特定のチーム プロジェクト内のコンテンツを変更できるようにする必要があります。

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>チーム プロジェクトに接続し、フォルダー マッピングを作成します。

任意のコンテンツをソース管理に追加する前に、チーム プロジェクトに接続し、ローカル コンピューターで、サーバー上のフォルダー構造およびファイル システムの間のマッピングを作成する必要があります。

**チーム プロジェクトに接続し、ローカル パスをマップするには**

1. 開発者のワークステーションでは、Visual Studio 2010 を開きます。
2. Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**です。

    > [!NOTE]
    > 既に TFS サーバーへの接続を構成した場合は、手順 3. ~ 6. を省略できます。
3. **チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**です。
4. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**です。
5. **Team Foundation Server の追加**ダイアログ ボックスでは、TFS インスタンスの詳細を提供し、をクリックして**OK**です。

    ![](adding-content-to-source-control/_static/image1.png)
6. **Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**です。
7. **チーム プロジェクトへ接続**ダイアログ ボックスで、接続するチームを選択する TFS インスタンスを選択はプロジェクト コレクションに追加するチーム プロジェクトを選択とクリック**接続**です。

    ![](adding-content-to-source-control/_static/image2.png)
8. **チーム エクスプ ローラー**  ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**です。

    ![](adding-content-to-source-control/_static/image3.png)
9. **ソース管理エクスプ ローラー**  タブで、をクリックして**マップされていない**です。

    ![](adding-content-to-source-control/_static/image4.png)
10. **マップ** ダイアログ ボックスで、**ローカル フォルダー**ボックス、またはを参照 (作成)、チーム プロジェクトのルート フォルダーとして機能し、をクリックするローカル フォルダー**マップ**です。

    ![](adding-content-to-source-control/_static/image5.png)
11. ソース ファイルをダウンロードするメッセージが表示されたら、クリックして**はい**です。

    ![](adding-content-to-source-control/_static/image6.png)

この時点では、開発者のワークステーション上のローカル フォルダーに、サーバー側、チーム プロジェクトのフォルダーをマップしておきます。 ローカル フォルダーの構造に、チーム プロジェクトからも、既存のコンテンツをダウンロードしました。 独自のコンテンツ コントロールを追加するソースを起動できます。

## <a name="add-projects-and-solutions-to-source-control"></a>プロジェクトおよびソリューションをソース管理に追加します。

プロジェクトおよびソリューションをソース管理に追加するには、まず、ローカル コンピューター上のチーム プロジェクトのマップされたフォルダーに移動する必要があります。 サーバーと、追加した内容を同期するコンテンツで確認できます。

**ソース管理にプロジェクトを追加するには**

1. 開発者のワークステーションでは、チーム プロジェクトのマップされたフォルダー構造内の適切な場所にプロジェクトおよびソリューションを移動します。

    > [!NOTE]
    > 多くの組織では、プロジェクトおよびソリューションをソース管理にどのように編成する必要があるための推奨されるアプローチがあります。 フォルダーを構造する方法のガイダンスについては、次を参照してください。[操作方法: 構造ソース コントロール フォルダー Team Foundation Server で](https://msdn.microsoft.com/library/bb668992.aspx)です。
2. Visual Studio 2010 でのソリューションを開きます。
3. **ソリューション エクスプ ローラー**  ウィンドウでは、ソリューションを右クリックし、をクリックして**ソリューションをソース管理に追加**です。

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > 場合によっては、組織が TFS では、構造体の内容を好きな方法によっては、ソース コードの編成方法きめの細かい制御を提供するには、個別にソース管理にプロジェクトを追加する必要があります。
4. いることを確認、**ソース管理エクスプ ローラー**  タブには、チーム プロジェクトのサーバーのフォルダー構造内に追加したコンテンツが表示されます。

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **ソース管理エクスプ ローラー**  タブには、ローカル ファイル システム上のマップされたフォルダーにソリューションを追加するためにこれ以上確認でコンテンツが表示されます。 マップされていない場所で、ソリューションであった場合は、TFS とローカル ファイル システムの両方でフォルダーの場所を指定するように求められます。
5. **ソース管理エクスプ ローラー**  タブの 、**フォルダー**  ウィンドウで、チーム プロジェクトを右クリックして (たとえば、 **ContactManager**)、をクリックして**チェックイン保留中の変更**です。
6. **チェックイン-ソース ファイル**ダイアログ ボックスで、コメントを入力し、をクリックして**チェックイン**です。

    ![](adding-content-to-source-control/_static/image9.png)

この時点で TFS でのソース管理にソリューションを追加しました。

## <a name="add-external-dependencies-to-source-control"></a>ソース管理への外部の依存関係を追加します。

プロジェクトまたはソリューションをソース管理に追加すると、ファイルやプロジェクトまたはソリューション内のフォルダーも追加されます。 ただし、多くの場合、プロジェクトおよびソリューションまた依存ローカル アセンブリは、正常に機能するように、外部の依存関係。 このようなすべてのリソースをソース管理に、チーム ビルドと、開発者チームの他のメンバーの両方を使用できますが、コードを正常にビルドを追加する必要があります。

たとえば、連絡先のマネージャーのサンプル ソリューションのフォルダー構造には、パッケージをという名前のフォルダーが含まれています。 ADO.NET Entity Framework 4.1 のアセンブリとサポートのさまざまなリソースが含まれます。 パッケージ フォルダーは、連絡先のマネージャー ソリューションの一部ではありませんが、ソリューションは指定されていない場合は正常にビルドされません。 チームは、ソリューションをビルドするビルドを有効にするには、ソース管理に、[パッケージ] フォルダーを追加する必要があります。

> [!NOTE]
> パッケージ フォルダーを含めることは、Visual Studio 2010 用の NuGet 拡張機能を使用して、ソリューションに、Entity Framework、または同様のリソースを追加するときの動作の一般的なです。


**ソース管理にプロジェクト以外のコンテンツを追加するには**

1. (たとえば、パッケージ フォルダー) を追加する項目が、ローカル ファイル システム上のマップされたフォルダー内の適切な場所であることを確認します。
2. Visual Studio 2010 での**チーム エクスプ ローラー**  ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**です。

    ![](adding-content-to-source-control/_static/image10.png)
3. **ソース管理エクスプ ローラー**  タブで、**フォルダー**ペインで、アイテムまたはアイテムを含むフォルダーを追加します。
4. クリックして、**項目をフォルダーに追加**ボタンをクリックします。

    ![](adding-content-to-source-control/_static/image11.png)
5. **ソース管理に追加** ダイアログ ボックスで、フォルダーまたはをクリックし、追加する項目を選択**次**です。

    ![](adding-content-to-source-control/_static/image12.png)
6. **項目を除外** タブで、自動的に除外された (たとえば、アセンブリ) をクリックして必要な項目を選択**項目を含める**です。

    ![](adding-content-to-source-control/_static/image13.png)
7. **追加する項目** タブで、ことを確認に含めるすべてのファイルは、をクリックして**完了**です。

    ![](adding-content-to-source-control/_static/image14.png)
8. **ソース管理エクスプ ローラー**ウィンドウで、をクリックして、**チェックイン**ボタンをクリックします。

    ![](adding-content-to-source-control/_static/image15.png)
9. **チェックイン-ソース ファイル**ダイアログ ボックスで、コメントを入力し、をクリックして**チェックイン**です。

この時点では、ソース管理にソリューションの外部の依存関係を追加しました。

## <a name="conclusion"></a>まとめ

このトピックでは、チーム プロジェクトに接続し、フォルダー構造にマップをソース管理にコンテンツを追加する方法について説明します。 ソース管理下にあるアイテムを操作する方法の詳細については、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)です。

次のトピックでは、 [Web 配置を TFS のビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)、TFS チーム ビルドをビルドして、ソリューションを配置するサーバーを準備する方法について説明します。

## <a name="further-reading"></a>関連項目

TFS でのソース管理の操作に関する包括的な情報は、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)です。

>[!div class="step-by-step"]
[前へ](creating-a-team-project-in-tfs.md)
[次へ](configuring-a-tfs-build-server-for-web-deployment.md)
