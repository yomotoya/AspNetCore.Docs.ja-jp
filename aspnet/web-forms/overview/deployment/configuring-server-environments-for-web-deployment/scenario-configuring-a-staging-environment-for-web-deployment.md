---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'シナリオ: Web 配置用のステージング環境の構成 |Microsoft ドキュメント'
author: jrjlee
description: このトピックでは、ステージング環境の一般的な web 展開シナリオについて説明しのような環境を設定するために完了する必要があるタスクについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3864559b0599091beeacb87e90e80a51285039df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>シナリオ: Web 配置用のステージング環境の構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ステージング環境の一般的な web 展開シナリオについて説明し、類似した環境を設定するために完了する必要があるタスクについて説明します。


多くの組織では、ステージング環境を使用して、web アプリケーションや web サイトの更新プログラムのプレビューします。 これは、方法により、組織内のユーザーを探索し、サイト"、"またはつまりは実稼働環境に展開する前に、新しい機能やコンテンツを確認します。 ステージング環境は現実的なプレビューを提供するために、可能な限り、実稼働環境をレプリケートするよう設計されています。 この種類のステージング環境には、通常、これらの特徴があります。

- 環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。
- 開発チームによってまたは自動的にチームがビルド サーバーは、アプリケーションを手動で配置する可能性があります。
- ユーザーまたはアプリケーションの展開プロセスのアカウントでは、ステージング サーバーに管理者特権を持っているやすくします。
- アプリケーションへの変更は、頻繁に展開されているため、環境をシングル ステップをサポートする必要があります。 または、展開を自動化します。

> [!NOTE]
> このチュートリアルの範囲を超えては複数のサーバーのスケール アウト データベースの配置です。 この領域の詳細についてを参照してください[SQL Server オンライン ブック](https://technet.microsoft.com/library/ms130214.aspx)です。


たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、Team Foundation Server (TFS) にお問い合わせください Manager ソリューションを管理します。 Rob Walters、TFS 管理者には、により、開発者は必要に応じてステージング環境にデプロイをトリガーするビルド定義が作成されます。

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

ほとんどの場合、されません必ずしもする最新のビルドをステージング環境にデプロイに注意してください。 代わりに、検証とテスト環境で検証を既に受けている特定のビルドを配置する可能性の高いずっとできました。

## <a name="solution-overview"></a>ソリューションの概要

このシナリオでは、展開の要件の分析から次の点を推測できます。

- ステージングの web サーバーは、管理者以外の展開をサポートする必要がありますので、展開を実行するユーザーまたはプロセスのアカウントのステージング サーバー上の管理者特権ことはできません。 そのため、リモート エージェントではなく、Web 配置のハンドラーを使用するステージング web サーバーを構成する必要があります。
- ステージング環境には、複数の web サーバーが含まれていますが、ため Web Farm Framework (WFF) を使用して、サーバー ファームを作成する必要がありますに 1 回のクリックまたは自動の展開をサポートする必要があります。 このアプローチを使用して、アプリケーションを 1 つの web サーバー (プライマリ サーバー) を展開できます。 また WFF がステージング環境の他のすべての web サーバー上の展開にレプリケートされます。
- ユーザーまたはプロセス アカウントの展開を実行するには、データベースを作成する権限がある場合があります。 そのため、アカウントを追加する必要があります、 **dbcreator**リモート アクセス展開をサポートするデータベース サーバーを構成するだけでなく、データベース サーバーのサーバー ロール。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [サーバー ファームを作成する Web Farm Framework で](creating-a-server-farm-with-the-web-farm-framework.md)です。 このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートできるように、WFF を使用して、サーバー ファームを構成する方法について説明します。
- [Web デプロイの発行の Web サーバーを構成する (Web Deploy ハンドラー)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。 このトピックでは、Web Deploy の発行をリモート エージェントのアプローチを使用して、クリーンな Windows Server 2008 R2 ビルドから起動をサポートする web サーバーを作成する方法について説明します。
- [Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。 このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

開発者の標準的なテスト環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のテスト環境構成](scenario-configuring-a-test-environment-for-web-deployment.md)です。 通常の運用環境を構成する方法については、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](scenario-configuring-a-test-environment-for-web-deployment.md)
> [次へ](scenario-configuring-a-production-environment-for-web-deployment.md)
