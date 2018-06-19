---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Web 配置のサーバー環境の構成 |Microsoft ドキュメント
author: jrjlee
description: このチュートリアルでは、1 回のクリック、または自動のサポート、web サイトを展開し、さまざまな異なるシナリオで発行するサーバー環境を設定する方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892297"
---
<a name="configuring-server-environments-for-web-deployment"></a>Web 配置のサーバー環境の構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このチュートリアルでは、サーバー環境に 1 回のクリック、または自動のサポート、web サイトを展開およびさまざまなさまざまなシナリオでの発行を設定する方法を示します。 チュートリアルには、展開シナリオ ベースの概要を提供すると共に、Web Farm Framework (WFF) のサーバー ファームを設定して固有のアプローチをサポートするために web サーバーを構成するように、さまざまなタスクの完了を順番に処理するトピックが含まれています。上位レベルのエンド ツー エンドのガイダンスです。
> 
> チュートリアルで説明されている Fabrikam, Inc. の展開シナリオを使用して[Enterprise Web 展開: シナリオの概要](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)例とネットワーク インフラストラクチャの参照ポイントとして。
> 
> これらのチュートリアルのイタリア語の翻訳を参照してください。 [ http://www.lucamorelli.it](http://www.lucamorelli.it)です。


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

最初のトピックでは、 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)、(Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールを使用して web アプリケーションを発行する際の主な方法について説明 2.0。 また、それぞれのアプローチにマップされるシナリオを識別します。 ここは、シナリオの各トピックは、完了する必要があるタスクの概要を示し、これらのタスクを完了するために使用する必要がありますのトピックを識別します。

説明されている分割プロジェクト ファイルのアプローチを使用しているかどうかは[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)をビルドし、最後のトピックでは、ソリューションの配置[ターゲット環境の配置プロパティを構成する](configuring-deployment-properties-for-a-target-environment.md)、別のインストール先の環境に配置するための環境に固有のプロジェクト ファイルを構成する方法について説明します。

## <a name="key-technologies"></a>主要なテクノロジ

このチュートリアルは、web 配置をサポートするためにこれらの製品およびテクノロジを使用する方法について説明します。

- IIS 7.5
- Web Deploy 2.x
- WFF 2.x
- IIS Web 管理サービス (WMSvc)

このチュートリアルは、Windows Server 2008 R2、SQL Server 2008 R2、ASP.NET 4.0 では、および ASP.NET MVC 3 の使用についても触れます。

## <a name="other-tutorials-in-this-series"></a>このシリーズの他のチュートリアル

これは、エンタープライズ規模の web 配置で 5 つのチュートリアルの一連の一部を形成します。 系列の他のチュートリアルは、次に示します。

- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)です。 この入門的な内容は、一連のチュートリアルのコンテキストの背景を提供します。 チュートリアルのシナリオについて説明し、どのタスクと、系列全体で説明したチュートリアルに適合する広範なアプリケーション ライフ サイクル管理 (ALM) プロセスを示しています。
- [Web、企業内の配置](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)です。 このチュートリアルでは、概念的な概要 Microsoft Build Engine (MSBuild) プロジェクト ファイルを Web 発行パイプライン、Web デプロイ、およびその他の関連するテクノロジを提供します。 これは、複雑な展開プロセスを管理するこれらのツールを一緒に使用する方法について説明します。
- [Web 配置を Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 このチュートリアルでは、特定のビルドの配置を手動でトリガーや継続的インテグレーション (CI) プロセスの一環として自動化された展開を含む、さまざまな展開シナリオをサポートするように、Team Foundation Server (TFS) を構成する方法について説明します。
- [企業の Web 配置を高度な](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)します。 このチュートリアルは複数の環境のデータベースの配置をカスタマイズする、ファイルとフォルダーの展開から除外および展開プロセス中に web アプリケーションをオフラインにするなどのさまざまな高度な展開タスクを実行する方法を説明します.

> [!div class="step-by-step"]
> [次へ](choosing-the-right-approach-to-web-deployment.md)
