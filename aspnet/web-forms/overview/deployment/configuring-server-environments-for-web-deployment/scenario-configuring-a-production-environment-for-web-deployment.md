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
ms.openlocfilehash: cdd13f96ddf08ff86b01ef9de17ea82cf038ab28
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a><span data-ttu-id="d840b-103">シナリオ: Web 配置用の実稼働環境の構成</span><span class="sxs-lookup"><span data-stu-id="d840b-103">Scenario: Configuring a Production Environment for Web Deployment</span></span>
====================
<span data-ttu-id="d840b-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d840b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d840b-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d840b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d840b-106">このトピックでは、運用環境の一般的な web 展開シナリオについて説明し、類似した環境を設定するために完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d840b-106">This topic describes a typical web deployment scenario for a production environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="d840b-107">実稼働環境では、web アプリケーションまたは web サイトの最終送信先です。</span><span class="sxs-lookup"><span data-stu-id="d840b-107">The production environment is the final destination for a web application or a website.</span></span> <span data-ttu-id="d840b-108">この段階では、アプリケーションはテストで、ステージング環境に配置されているされ、準備ができて"を有効にする"</span><span class="sxs-lookup"><span data-stu-id="d840b-108">By this point, your application has been through testing, has been deployed to a staging environment, and is ready to "go live."</span></span> <span data-ttu-id="d840b-109">運用環境の特性の性質と組織、対象者、およびその他の要因の多くのサイズ、web コンテンツの目的に従ってばらつきがします。</span><span class="sxs-lookup"><span data-stu-id="d840b-109">The characteristics of a production environment can vary widely according to the nature and purpose of your web content, the size of your organization, your target audience, and lots of other factors.</span></span> <span data-ttu-id="d840b-110">エンタープライズ規模のシナリオでは、これらの特性が実稼働環境にあります。</span><span class="sxs-lookup"><span data-stu-id="d840b-110">In an enterprise-scale scenario, the production environment may have these characteristics:</span></span>

- <span data-ttu-id="d840b-111">環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="d840b-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="d840b-112">環境がインターネットに接続する場合は、内部ネットワークから分離する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d840b-112">If the environment is Internet-facing, it's likely to be segregated from your internal network.</span></span> <span data-ttu-id="d840b-113">境界ネットワーク内の別のサブネットである可能性があります、別のドメインである可能性があります、まったく別のネットワーク インフラストラクチャである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d840b-113">It may be on a different subnet in a perimeter network, it may be on a different domain, and it may be on an entirely different network infrastructure.</span></span>
- <span data-ttu-id="d840b-114">開発者とビルド サーバー プロセス アカウントは、運用サーバーで管理者特権を持っている可能性がきわめてではありません。</span><span class="sxs-lookup"><span data-stu-id="d840b-114">Developers and build server process accounts are highly unlikely to have administrator privileges on the production servers.</span></span>
- <span data-ttu-id="d840b-115">アプリケーションへの変更は、テストまたはステージング環境の展開よりも頻度が低いごとに展開されます。</span><span class="sxs-lookup"><span data-stu-id="d840b-115">Changes to applications are deployed on a less frequent basis than test or staging deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="d840b-116">このチュートリアルの範囲を超えては複数のサーバーのスケール アウト データベースの配置です。</span><span class="sxs-lookup"><span data-stu-id="d840b-116">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="d840b-117">この領域の詳細についてを参照してください[SQL Server オンライン ブック](https://technet.microsoft.com/library/ms130214.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-117">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="d840b-118">たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、チーム ビルド サーバーには、ユーザーの連絡先のマネージャー ソリューションをビルドし、1 つの手順で、ステージング環境に展開できるようにするビルド定義が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d840b-118">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), a Team Build server includes build definitions that let users build the Contact Manager solution and deploy it to a staging environment in a single step.</span></span> <span data-ttu-id="d840b-119">アプリケーションのセキュリティ要件およびネットワーク インフラストラクチャによって課される制約により、実稼働環境に展開する準備が実稼働環境の管理者必要があります手動で実稼働 web サーバー上に web パッケージのコピーし、インポートインターネット インフォメーション サービス (IIS) マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="d840b-119">When the application is ready to be deployed to production, due to the constraints imposed by security requirements and the network infrastructure, the production environment administrator must manually copy the web package onto a production web server and import it through Internet Information Services (IIS) Manager.</span></span>

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a><span data-ttu-id="d840b-120">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="d840b-120">Solution Overview</span></span>

<span data-ttu-id="d840b-121">このシナリオでは、展開の要件の分析から次の点を推測できます。</span><span class="sxs-lookup"><span data-stu-id="d840b-121">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="d840b-122">により、セキュリティの制限事項と、ネットワークの構成では、1 回のクリックまたは自動の展開をサポートするために、運用環境を構成することはできません。</span><span class="sxs-lookup"><span data-stu-id="d840b-122">Due to security restrictions and the network configuration, you can't configure the production environment to support one-click or automated deployment.</span></span> <span data-ttu-id="d840b-123">オフラインの展開は、このシナリオでのみ実行可能なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="d840b-123">Offline deployment is the only viable approach in this scenario.</span></span>
- <span data-ttu-id="d840b-124">実稼働環境には、サーバー ファームを作成する Web Farm Framework (WFF) を使用できるように、複数の web サーバーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d840b-124">The production environment includes multiple web servers, so you can use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="d840b-125">このアプローチを使用してでは、管理者がのみを 1 つの web サーバー (プライマリ サーバー) 上にアプリケーションをインポートする必要があり、WFF が、展開、運用環境で他のすべての web サーバーにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="d840b-125">Using this approach, the administrator only needs to import the application onto one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the production environment.</span></span>

<span data-ttu-id="d840b-126">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="d840b-126">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="d840b-127">[サーバー ファームを作成する Web Farm Framework で](configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-127">[Create a Server Farm with the Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="d840b-128">このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートできるように、WFF を使用して、サーバー ファームを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d840b-128">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="d840b-129">[Web デプロイの発行 (オフライン展開) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-129">[Configure a Web Server for Web Deploy Publishing (Offline Deployment)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="d840b-130">このトピックでは、管理者は、インポートおよび web パッケージを手動で、展開 Windows Server 2008 R2 のクリーン ビルドからを開始できる web サーバーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d840b-130">This topic describes how to build a web server that lets administrators import and deploy web packages manually, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="d840b-131">[Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-131">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="d840b-132">このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d840b-132">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="d840b-133">関連項目</span><span class="sxs-lookup"><span data-stu-id="d840b-133">Further Reading</span></span>

<span data-ttu-id="d840b-134">開発者の標準的なテスト環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のテスト環境構成](scenario-configuring-a-test-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-134">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="d840b-135">標準的なステージング環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="d840b-135">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d840b-136">[前へ](scenario-configuring-a-staging-environment-for-web-deployment.md)
[次へ](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span><span class="sxs-lookup"><span data-stu-id="d840b-136">[Previous](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span></span>
