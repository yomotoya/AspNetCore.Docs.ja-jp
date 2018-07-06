---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'シナリオ: Web 配置を運用環境の構成 |Microsoft Docs'
author: jrjlee
description: このトピックでは、運用環境の一般的な web 展開シナリオと同様に設定するために完了する必要があるタスクについて説明します.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: dc16b2fadec8051f78d13ffeb97abf2d9e6930f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806714"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a><span data-ttu-id="8aac3-103">シナリオ: Web 配置を運用環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-103">Scenario: Configuring a Production Environment for Web Deployment</span></span>
====================
<span data-ttu-id="8aac3-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8aac3-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8aac3-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="8aac3-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8aac3-106">このトピックでは、運用環境の一般的な web 展開シナリオと類似した環境を設定するには完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-106">This topic describes a typical web deployment scenario for a production environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="8aac3-107">運用環境では、web アプリケーションまたは web サイトの最終的な宛先です。</span><span class="sxs-lookup"><span data-stu-id="8aac3-107">The production environment is the final destination for a web application or a website.</span></span> <span data-ttu-id="8aac3-108">この段階では、アプリケーション テストされました、ステージング環境に配置されている、「ライブ移動します」する準備ができました</span><span class="sxs-lookup"><span data-stu-id="8aac3-108">By this point, your application has been through testing, has been deployed to a staging environment, and is ready to "go live."</span></span> <span data-ttu-id="8aac3-109">運用環境の特性は、性質と、web コンテンツの目的で、組織、対象ユーザー、およびその他の要因の多くのサイズによって広く異なることができます。</span><span class="sxs-lookup"><span data-stu-id="8aac3-109">The characteristics of a production environment can vary widely according to the nature and purpose of your web content, the size of your organization, your target audience, and lots of other factors.</span></span> <span data-ttu-id="8aac3-110">エンタープライズ規模のシナリオでは、これらの特性が、実稼働環境にあります。</span><span class="sxs-lookup"><span data-stu-id="8aac3-110">In an enterprise-scale scenario, the production environment may have these characteristics:</span></span>

- <span data-ttu-id="8aac3-111">環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8aac3-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="8aac3-112">環境がインターネットに接続する場合は、内部ネットワークから分離するのには高くなります。</span><span class="sxs-lookup"><span data-stu-id="8aac3-112">If the environment is Internet-facing, it's likely to be segregated from your internal network.</span></span> <span data-ttu-id="8aac3-113">境界ネットワーク内の別のサブネットである可能性があります、別のドメインである可能性があり、まったく別のネットワーク インフラストラクチャである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8aac3-113">It may be on a different subnet in a perimeter network, it may be on a different domain, and it may be on an entirely different network infrastructure.</span></span>
- <span data-ttu-id="8aac3-114">開発者とビルド サーバー プロセスのアカウントでは、運用サーバーで管理者特権を持っている可能性が高くありません。</span><span class="sxs-lookup"><span data-stu-id="8aac3-114">Developers and build server process accounts are highly unlikely to have administrator privileges on the production servers.</span></span>
- <span data-ttu-id="8aac3-115">アプリケーションへの変更は、頻繁にテストまたはステージング環境のデプロイよりも上にデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="8aac3-115">Changes to applications are deployed on a less frequent basis than test or staging deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="8aac3-116">複数のサーバーをスケール アウト データベースの配置はこのチュートリアルの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="8aac3-116">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="8aac3-117">この領域の詳細についてを参照してください[SQL Server オンライン ブックの「](https://technet.microsoft.com/library/ms130214.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-117">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="8aac3-118">たとえば、[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、チーム ビルド サーバーには、ユーザーが連絡先マネージャー ソリューションをビルドし、1 つのステップでステージング環境に配置できるビルド定義が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8aac3-118">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), a Team Build server includes build definitions that let users build the Contact Manager solution and deploy it to a staging environment in a single step.</span></span> <span data-ttu-id="8aac3-119">運用環境の管理者の実稼働 web サーバー上に web パッケージをコピーしてインポートする必要があります手動でアプリケーションがセキュリティ要件と、ネットワーク インフラストラクチャによって課される制約のため、運用環境にデプロイする準備ができたときインターネット インフォメーション サービス (IIS) マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-119">When the application is ready to be deployed to production, due to the constraints imposed by security requirements and the network infrastructure, the production environment administrator must manually copy the web package onto a production web server and import it through Internet Information Services (IIS) Manager.</span></span>

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a><span data-ttu-id="8aac3-120">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="8aac3-120">Solution Overview</span></span>

<span data-ttu-id="8aac3-121">このシナリオでは、展開の要件の分析からこれらのファクトを推測できます。</span><span class="sxs-lookup"><span data-stu-id="8aac3-121">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="8aac3-122">により、セキュリティの制限事項とネットワークの構成では、1 回のクリック、または自動の展開をサポートする運用環境を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="8aac3-122">Due to security restrictions and the network configuration, you can't configure the production environment to support one-click or automated deployment.</span></span> <span data-ttu-id="8aac3-123">オフライン展開は、このシナリオでのみ実行可能なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="8aac3-123">Offline deployment is the only viable approach in this scenario.</span></span>
- <span data-ttu-id="8aac3-124">運用環境には、サーバー ファームを作成する Web Farm Framework (WFF) を使用できるように、複数の web サーバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8aac3-124">The production environment includes multiple web servers, so you can use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="8aac3-125">このアプローチを使用して、管理者がのみ 1 つの web サーバー (プライマリ サーバー) 上にアプリケーションをインポートする必要があり、WFF は運用環境でその他のすべての web サーバー上でデプロイをレプリケートします。</span><span class="sxs-lookup"><span data-stu-id="8aac3-125">Using this approach, the administrator only needs to import the application onto one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the production environment.</span></span>

<span data-ttu-id="8aac3-126">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-126">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="8aac3-127">[Web Farm Framework でサーバー ファームを作成する](configuring-a-database-server-for-web-deploy-publishing.md)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-127">[Create a Server Farm with the Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="8aac3-128">このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートされるように、WFF を使用してサーバー ファームを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-128">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="8aac3-129">[Web 配置発行 (オフライン展開) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-129">[Configure a Web Server for Web Deploy Publishing (Offline Deployment)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="8aac3-130">このトピックでは、管理者は、インポートし、web パッケージを手動で、展開、クリーン ビルドを Windows Server 2008 R2 から起動できる web サーバーを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-130">This topic describes how to build a web server that lets administrators import and deploy web packages manually, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="8aac3-131">[Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-131">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="8aac3-132">このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-132">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="8aac3-133">関連項目</span><span class="sxs-lookup"><span data-stu-id="8aac3-133">Further Reading</span></span>

<span data-ttu-id="8aac3-134">一般的な開発テスト環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-134">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="8aac3-135">一般的なステージング環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="8aac3-135">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8aac3-136">[前へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [次へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span><span class="sxs-lookup"><span data-stu-id="8aac3-136">[Previous](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span></span>
