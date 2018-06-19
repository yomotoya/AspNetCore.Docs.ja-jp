---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Web 配置を Team Foundation Server の構成 |Microsoft ドキュメント
author: jrjlee
description: このチュートリアルでは、ソリューションをビルドし、web コンテンツをさまざまなターゲット環境に配置を Team Foundation Server (TFS) 2010 を構成する方法を示します。 これ。。。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c4cfac333c9400d9ee613ba88520b0b0439873f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892648"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Web 配置を Team Foundation Server を構成します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、ソリューションをビルドし、web コンテンツをさまざまなターゲット環境に配置を Team Foundation Server (TFS) 2010 を構成する方法を示します。 これには、展開するコンテンツに自動的に、開発者によって変更するたびに、継続的インテグレーション (CI) のシナリオが含まれます。 手動トリガーのシナリオで、管理者は、ビルドを検証し、テスト環境で検証とステージング環境に固有のビルドの配置をトリガーする必要のある型を含めることもできます。 このチュートリアルのトピックに従って、構成プロセス全体を含みます。
> 
> - TFS で、新しいチーム プロジェクトを作成する方法。
> - ソース管理にコンテンツを追加する方法。
> - CI と展開をサポートするために、ビルド サーバーを構成する方法。
> - デプロイ ロジックを含むビルド定義を作成する方法。
> - 自動展開のアクセス許可を構成する方法。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。


このチュートリアルでは、TFS 2010 をインストールして、初期構成プロセスの一部としてチーム プロジェクト コレクションを作成したと仮定します。 [For Visual Studio 2010 Team Foundation のインストール ガイド](https://go.microsoft.com/?linkid=9805132)これらのタスクに関する包括的なガイダンスを示します。

## <a name="context"></a>コンテキスト

これが、エンタープライズ展開の要件に基づいて Fabrikam, Inc. という架空の会社のチュートリアルの一連の一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューション&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="scenario-overview"></a>シナリオの概要

これらのチュートリアルの高度なシナリオについては、「 [Enterprise Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)です。 このチュートリアルを始める前に、このトピックの内容を確認することをお勧めします。

## <a name="how-to-use-this-tutorial"></a>このチュートリアルを使用する方法

このチュートリアルで説明するタスクを実行した場合は、初めてまたはサンプル ソリューションを使用する例を実行する場合は、順序のチュートリアル トピックに従って操作する必要があります。 または、特定のタスクに対して、個々 のトピックをガイダンスとして使用できます。 このチュートリアルには、これらのトピックが含まれています。

- [TFS でチーム プロジェクトの作成](creating-a-team-project-in-tfs.md)です。 チーム プロジェクトは、ソース管理、プロセスの管理、および TFS でのビルドの中核となる単位です。 ソース管理またはビルド定義を作成するコンテンツを追加する前に、チーム プロジェクトを作成する必要があります。
- [ソース管理へのコンテンツの追加](adding-content-to-source-control.md)です。 チーム プロジェクトを作成したら、ソース管理へのコンテンツの追加を開始できます。 ビルドの構成を開始する前に、プロジェクトおよびソリューションの場合、外部の依存関係と共にを追加する必要があります。
- [Web 配置のビルド サーバーに、TFS を構成する](configuring-a-tfs-build-server-for-web-deployment.md)です。 チーム プロジェクトのコンテンツを構築する場合は、ビルド サーバーを構成する必要があります。 ほとんどの場合、TFS のインストールから別のコンピューターになります。 ビルド サーバーを構成するのには、インストールし TFS ビルド サービスの構成、Visual Studio 2010 をインストール、ビルド コント ローラーを作成し、ビルド エージェント、すべての製品やコードが正常にビルドしてインストールするために必要なコンポーネントをインストールする必要があります、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) します。
- [展開をサポートするビルド定義を作成する](creating-a-build-definition-that-supports-deployment.md)です。 キューまたは TFS でのビルドをトリガーするを開始するには、チーム プロジェクト用に少なくとも 1 つのビルド定義を作成する必要があります。 ビルド定義では、ビルド、ビルド内のどの部分を含める必要が、ビルドをトリガーする、チーム ビルドしてビルド出力を送信する必要がありますなどのすべての側面を定義します。 自動ビルドでデプロイ ロジックを含めることが可能、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを実行するためのビルド定義を構成することができます。
- [特定のビルドを配置](deploying-a-specific-build.md)です。 多数のシナリオでは、最新のビルドではなく、特定のビルドを対象となる環境に展開するします。 ここでは、特定のドロップ フォルダーからコンテンツを展開するビルド定義を構成することができます。
- [チームのアクセス許可の構成のビルドの配置](configuring-permissions-for-team-build-deployment.md)です。 コンテンツを展開する自動ビルド プロセスの一環としてビルド サービスがある場合、ビルド サービス アカウントに、移行先の web サーバーとデータベース サーバーにさまざまなアクセス許可を付与する必要があります。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルは、自動ビルドと web 配置をサポートするためにこれらの製品およびテクノロジを使用する方法について説明します。

- Visual Studio Team Foundation Server 2010
- チーム ビルドと MSBuild
- Web 配置

このチュートリアルは、Windows Server 2008 R2、IIS 7.5、SQL Server 2008 R2、ASP.NET 4.0 では、および ASP.NET MVC 3 の使用についても触れます。

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 配置で 5 つのチュートリアルの一連の一部を形成します。 系列の他のチュートリアルは、次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。 この入門的な内容は、一連のチュートリアルのコンテキストの背景を提供します。 チュートリアルのシナリオについて説明し、どのタスクと、系列全体で説明したチュートリアルに適合する広範なアプリケーション ライフ サイクル管理 (ALM) プロセスを示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 このチュートリアルでは、概念的な概要 MSBuild プロジェクト ファイルを Web 発行パイプライン (WPP)、Web デプロイ、およびその他の関連するテクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールを一緒に使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web デプロイのハンドラーおよびリモートのデータベースの配置を使用してリモートの web パッケージの配置を含むさまざまな展開シナリオをサポートする Windows サーバーを構成する方法について説明します。 ご使用の環境の適切な展開方法を選択するためのガイダンスを提供し、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー経由で展開された web アプリケーションをレプリケートする方法について説明します。
- [企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは複数の環境のデータベースの配置をカスタマイズする、ファイルとフォルダーの展開から除外および展開プロセス中に web アプリケーションをオフラインにするなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](creating-a-team-project-in-tfs.md)
