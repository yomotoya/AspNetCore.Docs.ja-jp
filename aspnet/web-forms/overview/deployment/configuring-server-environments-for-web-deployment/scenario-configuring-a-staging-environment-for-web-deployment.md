---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'シナリオ: Web 展開用のステージング環境の構成 |Microsoft Docs'
author: jrjlee
description: このトピックでは、ステージング環境の一般的な web 展開シナリオと同様の環境変数を設定するには完了する必要があるタスクについて説明します.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9c0f12645f10820996a03c232d20ee87031aefaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820802"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a><span data-ttu-id="4b03b-103">シナリオ: Web 展開用のステージング環境の構成</span><span class="sxs-lookup"><span data-stu-id="4b03b-103">Scenario: Configuring a Staging Environment for Web Deployment</span></span>
====================
<span data-ttu-id="4b03b-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="4b03b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="4b03b-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="4b03b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="4b03b-106">このトピックでは、ステージング環境の一般的な web 展開シナリオと同様の環境をセットアップするには完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-106">This topic describes a typical web deployment scenario for a staging environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="4b03b-107">多くの組織では、web アプリケーションまたは web サイトの更新をプレビューするのにステージング環境を使用します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-107">Lots of organizations use staging environments to preview updates to web applications or websites.</span></span> <span data-ttu-id="4b03b-108">これは、方法により、組織内のユーザーを探索し、サイトの「挿入、ライブ」またはつまりは運用環境にデプロイする前に、新しい機能やコンテンツを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4b03b-108">This gives people within the organization a chance to explore and review new functionality or content before the site "goes live," or in other words is deployed to a production environment.</span></span> <span data-ttu-id="4b03b-109">現実的なプレビューを提供するために、可能な限り、運用環境をレプリケートするは、ステージング環境は設計されています。</span><span class="sxs-lookup"><span data-stu-id="4b03b-109">The staging environment is designed to replicate the production environment as closely as possible, in order to provide a realistic preview.</span></span> <span data-ttu-id="4b03b-110">この種のステージング環境には、通常、これらの特徴があります。</span><span class="sxs-lookup"><span data-stu-id="4b03b-110">This kind of staging environment typically has these characteristics:</span></span>

- <span data-ttu-id="4b03b-111">環境は、複数の負荷分散された web サーバーおよびフェールオーバー クラスタ リングとデータベース ミラーリングで多くの場合、1 つまたは複数のデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="4b03b-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="4b03b-112">アプリケーションは、開発チームによってまたは自動的にチームのビルド サーバーで手動でデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="4b03b-112">Applications may be deployed manually by a development team or automatically by a Team Build server.</span></span>
- <span data-ttu-id="4b03b-113">ユーザーまたはアプリケーションの展開プロセスのアカウントでは、ステージング サーバーで管理者特権を持っているほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="4b03b-113">The users or process accounts that deploy applications are unlikely to have administrator privileges on the staging servers.</span></span>
- <span data-ttu-id="4b03b-114">アプリケーションへの変更、環境がシングル ステップをサポートする必要があるため、頻繁に展開または展開を自動化します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-114">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="4b03b-115">複数のサーバーをスケール アウト データベースの配置はこのチュートリアルの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="4b03b-115">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="4b03b-116">この領域の詳細についてを参照してください[SQL Server オンライン ブックの「](https://technet.microsoft.com/library/ms130214.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-116">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="4b03b-117">たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)、Team Foundation Server (TFS) は、連絡先マネージャー ソリューションを管理します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-117">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) manages the Contact Manager solution.</span></span> <span data-ttu-id="4b03b-118">TFS 管理者は、Rob Walters がにより、開発者は必要に応じて、ステージング環境へ配置をトリガーするビルド定義を作成します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-118">The TFS administrator, Rob Walters, has created a build definition that lets developers trigger a deployment to the staging environment as required.</span></span>

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="4b03b-119">ほとんどの場合、しません必ずしもするステージング環境に、最新のビルドをデプロイに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4b03b-119">Note that in most cases, you won't necessarily want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="4b03b-120">代わりに、検証とテスト環境での検証では既に行われている特定のビルドをデプロイする可能性の高い多くできました。</span><span class="sxs-lookup"><span data-stu-id="4b03b-120">Instead, you're a lot more likely to want to deploy a specific build that has already undergone validation and verification in the test environment.</span></span>

## <a name="solution-overview"></a><span data-ttu-id="4b03b-121">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="4b03b-121">Solution Overview</span></span>

<span data-ttu-id="4b03b-122">このシナリオでは、展開の要件の分析からこれらのファクトを推測できます。</span><span class="sxs-lookup"><span data-stu-id="4b03b-122">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="4b03b-123">展開を実行するユーザーまたはプロセスのアカウントは、ステージング web サーバーは、管理者以外の展開をサポートする必要がありますので、ステージング サーバーで管理者特権を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4b03b-123">The user or process account that performs the deployment won't have administrator privileges on the staging servers, so the staging web servers must support non-administrator deployment.</span></span> <span data-ttu-id="4b03b-124">そのため、リモート エージェントではなく、Web 配置ハンドラーを使用してステージング web サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b03b-124">As such, you'll need to configure the staging web servers to use the Web Deploy Handler rather than the remote agent.</span></span>
- <span data-ttu-id="4b03b-125">ステージング環境に複数の web サーバーが含まれていますが、ため Web Farm Framework (WFF) を使用して、サーバー ファームを作成する必要がありますに 1 回のクリック、または自動の展開をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b03b-125">The staging environment includes multiple web servers, but it needs to support one-click or automated deployment, so you'll need to use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="4b03b-126">このアプローチを使用して、1 つの web サーバー (プライマリ サーバー) にアプリケーションを展開して、WFF はステージング環境でその他のすべての web サーバー上でデプロイをレプリケートします。</span><span class="sxs-lookup"><span data-stu-id="4b03b-126">Using this approach, you can deploy an application to one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the staging environment.</span></span>
- <span data-ttu-id="4b03b-127">ユーザーまたはプロセス アカウントの展開を実行するには、データベースを作成するアクセス許可がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="4b03b-127">The user or process account that performs the deployment must have permissions to create databases.</span></span> <span data-ttu-id="4b03b-128">そのため、アカウントを追加する必要があります、 **dbcreator**リモート アクセス展開をサポートするデータベース サーバーを構成するだけでなく、データベース サーバーでサーバーの役割。</span><span class="sxs-lookup"><span data-stu-id="4b03b-128">As such, you'll need to add the account to the **dbcreator** server role on the database server, in addition to configuring the database server to support remote access and deployment.</span></span>

<span data-ttu-id="4b03b-129">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-129">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="4b03b-130">[Web Farm Framework でサーバー ファームを作成する](creating-a-server-farm-with-the-web-farm-framework.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-130">[Create a Server Farm with the Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md).</span></span> <span data-ttu-id="4b03b-131">このトピックでは、作成して、web platform 製品とコンポーネント、構成設定、および web サイトおよびアプリケーションが複数の負荷分散された web サーバー間でレプリケートされるように、WFF を使用してサーバー ファームを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-131">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="4b03b-132">[Web 配置発行の Web サーバーの構成 (Web 配置ハンドラー)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-132">[Configure a Web Server for Web Deploy Publishing (Web Deploy Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="4b03b-133">このトピックでは、Web Deploy の発行、リモート エージェントのアプローチを使用してから、クリーン ビルドを Windows Server 2008 R2 以降をサポートする web サーバーを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-133">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="4b03b-134">[Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-134">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="4b03b-135">このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-135">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="4b03b-136">関連項目</span><span class="sxs-lookup"><span data-stu-id="4b03b-136">Further Reading</span></span>

<span data-ttu-id="4b03b-137">一般的な開発テスト環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-137">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="4b03b-138">通常、実稼働環境の構成方法の詳細については、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="4b03b-138">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4b03b-139">[前へ](scenario-configuring-a-test-environment-for-web-deployment.md)
> [次へ](scenario-configuring-a-production-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="4b03b-139">[Previous](scenario-configuring-a-test-environment-for-web-deployment.md)
[Next](scenario-configuring-a-production-environment-for-web-deployment.md)</span></span>
