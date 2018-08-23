---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'シナリオ: Web 展開用のステージング環境の構成 |Microsoft Docs'
author: jrjlee
description: このトピックでは、ステージング環境の一般的な web 展開シナリオと同様の環境変数を設定するには完了する必要があるタスクについて説明します.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bee856c2ece6fda90ec35a87d111715def990603
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836173"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>シナリオ: Web 展開用のステージング環境の構成
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ステージング環境の一般的な web 展開シナリオと同様の環境をセットアップするには完了する必要があるタスクについて説明します。


多くの組織では、web アプリケーションまたは web サイトの更新をプレビューするのにステージング環境を使用します。 これは、方法により、組織内のユーザーを探索し、サイトの「挿入、ライブ」またはつまりは運用環境にデプロイする前に、新しい機能やコンテンツを確認してください。 現実的なプレビューを提供するために、可能な限り、運用環境をレプリケートするは、ステージング環境は設計されています。 この種のステージング環境には、通常、これらの特徴があります。

- 環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。
- アプリケーションは、開発チームによってまたは自動的にチームのビルド サーバーで手動でデプロイできます。
- ユーザーまたはアプリケーションの展開プロセスのアカウントでは、ステージング サーバーで管理者特権を持っているほとんどありません。
- アプリケーションへの変更、環境がシングル ステップをサポートする必要があるため、頻繁に展開または展開を自動化します。

> [!NOTE]
> 複数のサーバーをスケール アウト データベースの配置はこのチュートリアルの範囲外です。 この領域の詳細についてを参照してください[SQL Server オンライン ブックの「](https://technet.microsoft.com/library/ms130214.aspx)します。


たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、Team Foundation Server (TFS) は、連絡先マネージャー ソリューションを管理します。 TFS 管理者は、Rob Walters がにより、開発者は必要に応じて、ステージング環境へ配置をトリガーするビルド定義を作成します。

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

ほとんどの場合、しません必ずしもするステージング環境に、最新のビルドをデプロイに注意してください。 代わりに、検証とテスト環境での検証では既に行われている特定のビルドをデプロイする可能性の高い多くできました。

## <a name="solution-overview"></a>ソリューションの概要

このシナリオでは、展開の要件の分析からこれらのファクトを推測できます。

- 展開を実行するユーザーまたはプロセスのアカウントは、ステージング web サーバーは、管理者以外の展開をサポートする必要がありますので、ステージング サーバーで管理者特権を必要はありません。 そのため、リモート エージェントではなく、Web 配置ハンドラーを使用してステージング web サーバーを構成する必要があります。
- ステージング環境に複数の web サーバーが含まれていますが、ため Web Farm Framework (WFF) を使用して、サーバー ファームを作成する必要がありますに 1 回のクリック、または自動の展開をサポートする必要があります。 このアプローチを使用して、1 つの web サーバー (プライマリ サーバー) にアプリケーションを展開して、WFF はステージング環境でその他のすべての web サーバー上でデプロイをレプリケートします。
- ユーザーまたはプロセス アカウントの展開を実行するには、データベースを作成するアクセス許可がある場合があります。 そのため、アカウントを追加する必要があります、 **dbcreator**リモート アクセス展開をサポートするデータベース サーバーを構成するだけでなく、データベース サーバーでサーバーの役割。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [Web Farm Framework でサーバー ファームを作成する](creating-a-server-farm-with-the-web-farm-framework.md)します。 このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートされるように、WFF を使用してサーバー ファームを構成する方法について説明します。
- [Web 配置発行の Web サーバーの構成 (Web 配置ハンドラー)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)します。 このトピックでは、Web Deploy の発行、リモート エージェントのアプローチを使用してから、クリーン ビルドを Windows Server 2008 R2 以降をサポートする web サーバーを構築する方法について説明します。
- [Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。 このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

一般的な開発テスト環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。 通常、実稼働環境の構成方法の詳細については、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](scenario-configuring-a-test-environment-for-web-deployment.md)
> [次へ](scenario-configuring-a-production-environment-for-web-deployment.md)
