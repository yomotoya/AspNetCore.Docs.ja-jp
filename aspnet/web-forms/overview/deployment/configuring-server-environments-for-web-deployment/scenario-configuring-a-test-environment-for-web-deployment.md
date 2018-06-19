---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'シナリオ: Web 配置用のテスト環境の構成 |Microsoft ドキュメント'
author: jrjlee
description: このトピックは、開発者の一般的な web 展開シナリオについて説明し、テスト環境または、si を設定するために完了する必要があるタスクについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879856"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>シナリオ: Web 配置用のテスト環境の構成
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、開発者の一般的な web 展開シナリオについて説明します、テスト環境しまたは類似した環境を設定するために完了する必要があるタスクについて説明します。


開発者は、web アプリケーションで作業、現実的な設定で、アプリケーションへの変更をテストする使用可能なサーバー環境に多くの場合、アクセスで与えられます。 このような開発環境またはテスト環境には、通常、これらの特徴があります。

- 環境は、1 つの web サーバーと 1 つのデータベース サーバーで構成されます。
- 開発者は、通常、管理者権限をアプリケーションの要件に環境を構成できるように、サーバー上。
- アプリケーションへの変更は、頻繁に展開されているため、環境をシングル ステップをサポートする必要があります。 または、展開を自動化します。

たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)Matt 明 Fabrikam, Inc. の開発者は、Matt は勤務先で連絡先のマネージャー ソリューションと定期的に変更をテスト環境に配置する必要があります。 Matt は、テストの web サーバーと、テスト データベース サーバーの管理者です。 最初に、Matt ができるように、ソリューションを直接、テスト環境を配置する必要があります。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

作業の進行状況と開発者の方ソリューションが継続的インテグレーション (CI) Team Foundation Server (TFS) で構成されている連絡先のマネージャー、チームに参加します。 開発者がコンテンツにチェックインされるたびにチーム ビルドする必要がありますソリューションをビルド、他の単体テストを実行し、自動的にソリューションをテスト環境にデプロイします。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>ソリューションの概要

テスト環境では、1 ステップをサポートする必要があります。 または 2 つの主な方法があるため、リモート コンピューターからのデプロイを自動化します。 次の操作を行うことができます。

- Web Deployment Agent サービス (「リモート エージェント」) を使用して展開をサポートするために、テストの web サーバーを構成します。
- Web Deploy ハンドラーを使用して展開をサポートするテストの web サーバーを構成します。

> [!NOTE]
> 使用することも[Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)(「一時エージェント」) です。 要件と制約の観点からリモート エージェント方法に似ています。


この場合、開発者は、移行先サーバーに管理者特権を持っているし、テスト環境は厳格なセキュリティ制約を前提と、論理的な選択では、リモート エージェントを使用して展開をサポートするテストの web サーバーを構成します。 これはそれほど複雑であり、Web 配置ハンドラー アプローチよりも小さい初期構成が必要です。 また、リモート アクセス展開をサポートするデータベース サーバーを構成する必要があります。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [Web デプロイの発行 (リモート エージェント) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。 このトピックでは、Web Deploy の発行をリモート エージェントのアプローチを使用して、クリーンな Windows Server 2008 R2 ビルドから起動をサポートする web サーバーを作成する方法について説明します。
- [Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。 このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

標準的なステージング環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。 通常の運用環境を構成する方法については、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](choosing-the-right-approach-to-web-deployment.md)
> [次へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
