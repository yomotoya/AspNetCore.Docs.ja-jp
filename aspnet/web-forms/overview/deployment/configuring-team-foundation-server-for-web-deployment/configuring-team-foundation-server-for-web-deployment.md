---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Web デプロイ用の Team Foundation Server の構成 |Microsoft Docs
author: jrjlee
description: このチュートリアルでは、ソリューションを構築し、さまざまなターゲット環境に web コンテンツをデプロイする Team Foundation Server (TFS) 2010 を構成する方法を説明します。 これ。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6430a96a8e430a8a30d062ec22868de829680806
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365342"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Web 配置の Team Foundation Server を構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、ソリューションを構築し、さまざまなターゲット環境に web コンテンツをデプロイする Team Foundation Server (TFS) 2010 を構成する方法を説明します。 これには、展開するコンテンツに自動的に、開発者が変更を行うたびに、継続的インテグレーション (CI) のシナリオが含まれます。 手動トリガーのシナリオで、管理者は、ビルドを検証し、テスト環境で検証したら、ステージング環境への特定のビルドの配置をトリガーする必要のある型を含めることもできます。 このチュートリアルでは、トピックのガイド全体の構成手順を含みます。
> 
> - TFS で新しいチーム プロジェクトを作成する方法。
> - ソース管理にコンテンツを追加する方法。
> - CI および展開をサポートするビルド サーバーを構成する方法。
> - デプロイ ロジックを含むビルド定義を作成する方法。
> - 自動展開のアクセス許可を構成する方法。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)します。


このチュートリアルでは、TFS 2010 のインストールし、初期構成プロセスの一部としてチーム プロジェクト コレクションの作成が前提としています。 [For Visual Studio 2010 Team Foundation インストール ガイド](https://go.microsoft.com/?linkid=9805132)これらのタスクの包括的なガイダンスを提供します。

## <a name="context"></a>コンテキスト

これが Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づくチュートリアルのシリーズの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、 [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)ソリューション&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="scenario-overview"></a>シナリオの概要

これらのチュートリアルの高度なシナリオについては、「[エンタープライズ Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)します。 このトピックでは、このチュートリアルを開始する前に確認することをお勧めします。

## <a name="how-to-use-this-tutorial"></a>このチュートリアルを使用する方法

このチュートリアルで説明するタスクを実行した場合、これは、最初の時間またはサンプル ソリューションを使用してサンプルを実行する場合は、チュートリアルのトピックでは、順序で作業する必要があります。 または、特定のタスクを個々 のトピックをガイドとして使用できます。 このチュートリアルには、これらのトピックが含まれています。

- [TFS でチーム プロジェクトを作成する](creating-a-team-project-in-tfs.md)します。 チーム プロジェクトは、ソース管理、プロセスの管理、および TFS でビルドの中核となる単位です。 ソース管理またはビルド定義を作成するコンテンツを追加する前に、チーム プロジェクトを作成する必要があります。
- [ソース管理へのコンテンツの追加](adding-content-to-source-control.md)します。 チーム プロジェクトを作成したら、ソース管理へのコンテンツの追加を開始できます。 ビルドの構成を開始する前に、プロジェクトや、外部の依存関係と共に、ソリューションを追加する必要があります。
- [Web 配置のビルド サーバーを TFS を構成する](configuring-a-tfs-build-server-for-web-deployment.md)します。 チーム プロジェクトのコンテンツを構築する場合は、ビルド サーバーを構成する必要があります。 ほとんどの場合、TFS のインストールから別のコンピューターにあります。 ビルド サーバーを構成するには、インストールし TFS ビルド サービスの構成、Visual Studio 2010 をインストール、ビルド コント ローラーを作成し、ビルド エージェント、製品やコードに正常にビルドしてインストールするために必要なコンポーネントをインストールする必要があります、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置)。
- [展開をサポートするビルド定義を作成する](creating-a-build-definition-that-supports-deployment.md)します。 キューまたは TFS でビルドをトリガーするを開始するには、チーム プロジェクト用に少なくとも 1 つのビルド定義を作成する必要があります。 ビルド定義では、のどの部分は、ビルドに含める必要がどのようなビルドをトリガーする必要があります、チーム ビルドしてビルド出力を送信する必要があります場所など、ビルドのすべての側面を定義します。 自動化されたビルドで、デプロイ ロジックを含めることができますが、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを実行するビルド定義を構成することができます。
- [特定のビルドを展開する](deploying-a-specific-build.md)します。 多数のシナリオでは、ターゲット環境を最新のビルドではなく、特定のビルドをデプロイするします。 この場合は、特定のドロップ フォルダーからコンテンツを展開するビルド定義を構成できます。
- [チームのアクセス許可の構成のビルドの配置](configuring-permissions-for-team-build-deployment.md)します。 ビルド サービスが、自動化されたビルド プロセスの一部としてコンテンツをデプロイする場合は、任意の変換先の web サーバーとデータベース サーバー、ビルド サービス アカウントにさまざまなアクセス許可を付与する必要があります。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルでは、これらの製品やテクノロジを使用して、自動化されたビルドと web 配置をサポートする方法について説明します。

- Visual Studio Team Foundation Server 2010
- チーム ビルドと MSBuild
- Web 配置

このチュートリアルは、Windows Server 2008 R2、IIS 7.5、SQL Server 2008 R2、ASP.NET 4.0 では、ASP.NET MVC 3 の使用についても触れます。

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 展開の 5 つのチュートリアルのシリーズの一部を形成します。 他のチュートリアル シリーズの次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。 この概要のコンテンツは、チュートリアル シリーズのコンテキストの背景を提供します。 このチュートリアルのシナリオについて説明し、タスクとチュートリアル シリーズで説明されているが、広範なアプリケーション ライフ サイクル管理 (ALM) のプロセスに適合させる方法を示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 このチュートリアルでは、概念的な概要 MSBuild プロジェクト ファイルを Web 発行パイプライン (WPP)、Web デプロイ、およびその他の関連テクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールをまとめて使用する方法について説明します。
- [Web 配置のサーバー環境を構成する](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。 このチュートリアルでは、Web Deployment Agent サービス (リモート エージェント) または Web 配置ハンドラーとリモートのデータベースの配置を使用して、リモート web パッケージ展開を含め、さまざまなデプロイメント シナリオをサポートするために Windows サーバーを構成する方法について説明します。 独自の環境の適切な展開方法の選択について説明して、Web Farm Framework (WFF) を使用して、サーバー ファーム内のすべての web サーバー間でデプロイされた web アプリケーションをレプリケートする方法について説明します。
- [エンタープライズ Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、複数の環境のデータベースの展開をカスタマイズ、展開からのファイルとフォルダーの除外、展開プロセス中に web アプリケーションをオフラインすることなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](creating-a-team-project-in-tfs.md)
