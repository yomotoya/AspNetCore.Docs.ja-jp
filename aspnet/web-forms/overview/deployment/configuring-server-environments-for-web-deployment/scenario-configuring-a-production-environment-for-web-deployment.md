---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'シナリオ: Web 配置を運用環境の構成 |Microsoft Docs'
author: jrjlee
description: このトピックでは、運用環境の一般的な web 展開シナリオと同様に設定するために完了する必要があるタスクについて説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff9a1e7657852f37b3dc4fc1dbc4f6e78e6427cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370349"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>シナリオ: Web 配置を運用環境を構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、運用環境の一般的な web 展開シナリオと類似した環境を設定するには完了する必要があるタスクについて説明します。


運用環境では、web アプリケーションまたは web サイトの最終的な宛先です。 この段階では、アプリケーション テストされました、ステージング環境に配置されている、「ライブ移動します」する準備ができました 運用環境の特性は、性質と、web コンテンツの目的で、組織、対象ユーザー、およびその他の要因の多くのサイズによって広く異なることができます。 エンタープライズ規模のシナリオでは、これらの特性が、実稼働環境にあります。

- 環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。
- 環境がインターネットに接続する場合は、内部ネットワークから分離するのには高くなります。 境界ネットワーク内の別のサブネットである可能性があります、別のドメインである可能性があり、まったく別のネットワーク インフラストラクチャである可能性があります。
- 開発者とビルド サーバー プロセスのアカウントでは、運用サーバーで管理者特権を持っている可能性が高くありません。
- アプリケーションへの変更は、頻繁にテストまたはステージング環境のデプロイよりも上にデプロイされます。

> [!NOTE]
> 複数のサーバーをスケール アウト データベースの配置はこのチュートリアルの範囲外です。 この領域の詳細についてを参照してください[SQL Server オンライン ブックの「](https://technet.microsoft.com/library/ms130214.aspx)します。


たとえば、[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、チーム ビルド サーバーには、ユーザーが連絡先マネージャー ソリューションをビルドし、1 つのステップでステージング環境に配置できるビルド定義が含まれています。 運用環境の管理者の実稼働 web サーバー上に web パッケージをコピーしてインポートする必要があります手動でアプリケーションがセキュリティ要件と、ネットワーク インフラストラクチャによって課される制約のため、運用環境にデプロイする準備ができたときインターネット インフォメーション サービス (IIS) マネージャーを使用します。

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>ソリューションの概要

このシナリオでは、展開の要件の分析からこれらのファクトを推測できます。

- により、セキュリティの制限事項とネットワークの構成では、1 回のクリック、または自動の展開をサポートする運用環境を構成することはできません。 オフライン展開は、このシナリオでのみ実行可能なアプローチです。
- 運用環境には、サーバー ファームを作成する Web Farm Framework (WFF) を使用できるように、複数の web サーバーが含まれます。 このアプローチを使用して、管理者がのみ 1 つの web サーバー (プライマリ サーバー) 上にアプリケーションをインポートする必要があり、WFF は運用環境でその他のすべての web サーバー上でデプロイをレプリケートします。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [Web Farm Framework でサーバー ファームを作成する](configuring-a-database-server-for-web-deploy-publishing.md)します。 このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートされるように、WFF を使用してサーバー ファームを構成する方法について説明します。
- [Web 配置発行 (オフライン展開) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。 このトピックでは、管理者は、インポートし、web パッケージを手動で、展開、クリーン ビルドを Windows Server 2008 R2 から起動できる web サーバーを構築する方法について説明します。
- [Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。 このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

一般的な開発テスト環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。 一般的なステージング環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
