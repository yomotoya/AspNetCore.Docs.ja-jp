---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Web 配置のサーバー環境の構成 |Microsoft Docs
author: jrjlee
description: このチュートリアルでは、1 回のクリック、または自動のサポート、web サイトの配置、およびさまざまな異なるシナリオで発行するサーバー環境を設定する方法を説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4f6433f0b8a9ad3b3634c9bcd8d95015eebaa865
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827684"
---
<a name="configuring-server-environments-for-web-deployment"></a>Web 配置のサーバー環境を構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、1 回のクリック、または自動のサポート、web サイトの配置、およびさまざまなさまざまなシナリオで発行するサーバー環境を設定する方法を説明します。 このチュートリアルには、デプロイとシナリオ ベースの概要を提供すると共に、Web Farm Framework (WFF) のサーバー ファームをセットアップする具体的なアプローチをサポートするために web サーバーの構成など、さまざまなタスクを完了することを説明するトピックが含まれています。高度なエンド ツー エンドのガイダンスです。
> 
> チュートリアルで説明されている Fabrikam, Inc. の展開シナリオを使用して[エンタープライズ Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)例とネットワーク インフラストラクチャの参照ポイントとして。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)します。


このチュートリアルには、これらのトピックが含まれています。

- [適切な Web 展開手法を選択する](choosing-the-right-approach-to-web-deployment.md)
- [シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)
- [シナリオ: Web 展開のステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [シナリオ: Web 展開の運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Web 配置発行の Web サーバーを構成する (リモート エージェント)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Web 配置発行の Web サーバーを構成する (Web 配置ハンドラー)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Web 配置発行の Web サーバーを構成する (オフライン展開)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Web 配置発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)
- [Web Farm Framework でサーバー ファームを作成する](creating-a-server-farm-with-the-web-farm-framework.md)
- [ターゲット環境の展開プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)

最初のトピックでは、 [Web 配置を右側のアプローチを選択](choosing-the-right-approach-to-web-deployment.md)、インターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用して web アプリケーションを発行する際の主な方法について説明 2.0。 また、各アプローチにマップするシナリオを識別します。 ここは、各シナリオのトピックでは、完了する必要があるタスクの概要を示し、これらのタスクを完了するのに役立つを使用する必要がありますのトピックを識別します。

説明されている分割プロジェクト ファイルの方法を使用しているかどうかは[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)をビルドし、最後のトピックでは、ソリューションのデプロイ[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)、別の変換先の環境にデプロイするための環境に固有のプロジェクト ファイルを構成する方法について説明します。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルでは、これらの製品やテクノロジを使用して、web 配置をサポートする方法について説明します。

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- IIS Web 管理サービス (WMSvc)

このチュートリアルは、Windows Server 2008 R2、SQL Server 2008 R2、ASP.NET 4.0 では、ASP.NET MVC 3 の使用についても触れます。

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 展開の 5 つのチュートリアルのシリーズの一部を形成します。 他のチュートリアル シリーズの次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)します。 この概要のコンテンツは、チュートリアル シリーズのコンテキストの背景を提供します。 このチュートリアルのシナリオについて説明し、タスクとチュートリアル シリーズで説明されているが、広範なアプリケーション ライフ サイクル管理 (ALM) のプロセスに適合させる方法を示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)します。 このチュートリアルでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルの概念、Web 発行パイプライン、Web デプロイ、およびその他の関連テクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールをまとめて使用する方法について説明します。
- [Web 配置の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 このチュートリアルでは、継続的インテグレーション (CI) プロセスの一環として自動化された展開を含め、特定のビルドの展開を手動でトリガーされる、さまざまなデプロイ シナリオをサポートするように、Team Foundation Server (TFS) を構成する方法について説明します。
- [エンタープライズ Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは、複数の環境のデータベースの展開をカスタマイズ、展開からのファイルとフォルダーの除外、展開プロセス中に web アプリケーションをオフラインすることなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](choosing-the-right-approach-to-web-deployment.md)
