---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: "シナリオ: Web 配置用の実稼働環境の構成 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、運用環境のための一般的な web 展開シナリオを説明し、同様を設定するために完了する必要があるタスクについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d5574ee353ff41205e9029e4aa5d139a5aa0e959
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>シナリオ: Web 配置用の実稼働環境の構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、運用環境の一般的な web 展開シナリオについて説明し、類似した環境を設定するために完了する必要があるタスクについて説明します。


実稼働環境では、web アプリケーションまたは web サイトの最終送信先です。 この段階では、アプリケーションはテストで、ステージング環境に配置されているされ、準備ができて"を有効にする" 運用環境の特性の性質と組織、対象者、およびその他の要因の多くのサイズ、web コンテンツの目的に従ってばらつきがします。 エンタープライズ規模のシナリオでは、これらの特性が実稼働環境にあります。

- 環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。
- 環境がインターネットに接続する場合は、内部ネットワークから分離する可能性があります。 境界ネットワーク内の別のサブネットである可能性があります、別のドメインである可能性があります、まったく別のネットワーク インフラストラクチャである可能性があります。
- 開発者とビルド サーバー プロセス アカウントは、運用サーバーで管理者特権を持っている可能性がきわめてではありません。
- アプリケーションへの変更は、テストまたはステージング環境の展開よりも頻度が低いごとに展開されます。

> [!NOTE]
> このチュートリアルの範囲を超えては複数のサーバーのスケール アウト データベースの配置です。 この領域の詳細についてを参照してください[SQL Server オンライン ブック](https://technet.microsoft.com/en-us/library/ms130214.aspx)です。


たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、チーム ビルド サーバーには、ユーザーの連絡先のマネージャー ソリューションをビルドし、1 つの手順で、ステージング環境に展開できるようにするビルド定義が含まれています。 アプリケーションのセキュリティ要件およびネットワーク インフラストラクチャによって課される制約により、実稼働環境に展開する準備が実稼働環境の管理者必要があります手動で実稼働 web サーバー上に web パッケージのコピーし、インポートインターネット インフォメーション サービス (IIS) マネージャーを使用します。

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>ソリューションの概要

このシナリオでは、展開の要件の分析から次の点を推測できます。

- により、セキュリティの制限事項と、ネットワークの構成では、1 回のクリックまたは自動の展開をサポートするために、運用環境を構成することはできません。 オフラインの展開は、このシナリオでのみ実行可能なアプローチです。
- 実稼働環境には、サーバー ファームを作成する Web Farm Framework (WFF) を使用できるように、複数の web サーバーが含まれます。 このアプローチを使用してでは、管理者がのみを 1 つの web サーバー (プライマリ サーバー) 上にアプリケーションをインポートする必要があり、WFF が、展開、運用環境で他のすべての web サーバーにレプリケートされます。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [サーバー ファームを作成する Web Farm Framework で](configuring-a-database-server-for-web-deploy-publishing.md)です。 このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートできるように、WFF を使用して、サーバー ファームを構成する方法について説明します。
- [Web デプロイの発行 (オフライン展開) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)です。 このトピックでは、管理者は、インポートおよび web パッケージを手動で、展開 Windows Server 2008 R2 のクリーン ビルドからを開始できる web サーバーを作成する方法について説明します。
- [Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。 このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

開発者の標準的なテスト環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のテスト環境構成](scenario-configuring-a-test-environment-for-web-deployment.md)です。 標準的なステージング環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。

>[!div class="step-by-step"]
[前へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
[次へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
