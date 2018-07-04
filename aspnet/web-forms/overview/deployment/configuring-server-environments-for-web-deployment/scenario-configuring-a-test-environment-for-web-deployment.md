---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'シナリオ: Web 展開用のテスト環境の構成 |Microsoft Docs'
author: jrjlee
description: このトピックでは、開発者の一般的な web 展開シナリオについて説明し、テスト環境または si を設定するには完了する必要があるタスクについて説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382495"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>シナリオ: Web 展開のテスト環境を構成します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、開発者の一般的な web 展開シナリオについて説明しますまたはテスト環境とのような環境をセットアップするには完了する必要があるタスクについて説明します。


開発者は、web アプリケーションで作業、現実的な設定で、アプリケーションに変更をテストに使用できるサーバー環境にアクセスで多くの場合、与えられます。 この種の開発またはテスト環境には、通常、これらの特徴があります。

- 環境は、1 つの web サーバーと 1 つのデータベース サーバーで構成されます。
- 通常、開発者は、アプリケーションの要件に環境を構成できるように、サーバー上で管理者特権を持ちます。
- アプリケーションへの変更、環境がシングル ステップをサポートする必要があるため、頻繁に展開または展開を自動化します。

たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)Matt 明が Fabrikam, inc. の開発者Matt は、連絡先マネージャー ソリューションが機能して、定期的に変更をテスト環境にデプロイする必要があります。 Matt は、テストの web サーバーとテストのデータベース サーバーの管理者です。 最初に、Matt は、直接ソリューションをテスト環境にデプロイできる必要があります。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

作業として進行しより多くの開発者、チームでは、ソリューションが継続的インテグレーション (CI) Team Foundation Server (TFS) で構成されている連絡先マネージャーに参加します。 開発者がコンテンツをチェックインするたびにチーム ビルドする必要がありますソリューションをビルド、単体テストを実行およびソリューションをテスト環境を自動的に配置します。

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>ソリューションの概要

テスト環境では、シングル ステップをサポートする必要があります。 または主に 2 つのアプローチを選択できるように、リモート コンピューターからのデプロイを自動化します。 次の操作を行うことができます。

- Web Deployment Agent サービス (「リモート エージェント」) を使用して展開をサポートするテスト web サーバーを構成します。
- Web 配置ハンドラーを使用して展開をサポートするテスト web サーバーを構成します。

> [!NOTE]
> 使用することも[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)(「一時エージェント」)。 要件と制約の観点から、リモート エージェントのアプローチに似ています。


この場合は、開発者は、移行先サーバーで管理者特権を持ってし、テスト環境では厳密なセキュリティ制約には、リモート エージェントを使用して展開をサポートするテスト web サーバーを構成するため、論理的な選択です。 これはそれほど複雑であり、Web 配置ハンドラー アプローチよりも少ない初期の構成が必要です。 また、リモート アクセス展開をサポートするデータベース サーバーを構成する必要もあります。

これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。

- [Web 配置発行 (リモート エージェント) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)します。 このトピックでは、Web Deploy の発行、リモート エージェントのアプローチを使用してから、クリーン ビルドを Windows Server 2008 R2 以降をサポートする web サーバーを構築する方法について説明します。
- [Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。 このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。

## <a name="further-reading"></a>関連項目

一般的なステージング環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。 通常、実稼働環境の構成方法の詳細については、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](choosing-the-right-approach-to-web-deployment.md)
> [次へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
