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
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="8020a-103">シナリオ: Web 配置用のテスト環境の構成</span><span class="sxs-lookup"><span data-stu-id="8020a-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="8020a-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8020a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8020a-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8020a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8020a-106">このトピックでは、開発者の一般的な web 展開シナリオについて説明します、テスト環境しまたは類似した環境を設定するために完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8020a-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="8020a-107">開発者は、web アプリケーションで作業、現実的な設定で、アプリケーションへの変更をテストする使用可能なサーバー環境に多くの場合、アクセスで与えられます。</span><span class="sxs-lookup"><span data-stu-id="8020a-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="8020a-108">このような開発環境またはテスト環境には、通常、これらの特徴があります。</span><span class="sxs-lookup"><span data-stu-id="8020a-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="8020a-109">環境は、1 つの web サーバーと 1 つのデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="8020a-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="8020a-110">開発者は、通常、管理者権限をアプリケーションの要件に環境を構成できるように、サーバー上。</span><span class="sxs-lookup"><span data-stu-id="8020a-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="8020a-111">アプリケーションへの変更は、頻繁に展開されているため、環境をシングル ステップをサポートする必要があります。 または、展開を自動化します。</span><span class="sxs-lookup"><span data-stu-id="8020a-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="8020a-112">たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)Matt 明 Fabrikam, Inc. の開発者は、Matt は勤務先で連絡先のマネージャー ソリューションと定期的に変更をテスト環境に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8020a-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="8020a-113">Matt は、テストの web サーバーと、テスト データベース サーバーの管理者です。</span><span class="sxs-lookup"><span data-stu-id="8020a-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="8020a-114">最初に、Matt ができるように、ソリューションを直接、テスト環境を配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8020a-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="8020a-115">作業の進行状況と開発者の方ソリューションが継続的インテグレーション (CI) Team Foundation Server (TFS) で構成されている連絡先のマネージャー、チームに参加します。</span><span class="sxs-lookup"><span data-stu-id="8020a-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="8020a-116">開発者がコンテンツにチェックインされるたびにチーム ビルドする必要がありますソリューションをビルド、他の単体テストを実行し、自動的にソリューションをテスト環境にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="8020a-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="8020a-117">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="8020a-117">Solution Overview</span></span>

<span data-ttu-id="8020a-118">テスト環境では、1 ステップをサポートする必要があります。 または 2 つの主な方法があるため、リモート コンピューターからのデプロイを自動化します。</span><span class="sxs-lookup"><span data-stu-id="8020a-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="8020a-119">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="8020a-119">You can:</span></span>

- <span data-ttu-id="8020a-120">Web Deployment Agent サービス (「リモート エージェント」) を使用して展開をサポートするために、テストの web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8020a-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="8020a-121">Web Deploy ハンドラーを使用して展開をサポートするテストの web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8020a-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="8020a-122">使用することも[Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)(「一時エージェント」) です。</span><span class="sxs-lookup"><span data-stu-id="8020a-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="8020a-123">要件と制約の観点からリモート エージェント方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="8020a-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="8020a-124">この場合、開発者は、移行先サーバーに管理者特権を持っているし、テスト環境は厳格なセキュリティ制約を前提と、論理的な選択では、リモート エージェントを使用して展開をサポートするテストの web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="8020a-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="8020a-125">これはそれほど複雑であり、Web 配置ハンドラー アプローチよりも小さい初期構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="8020a-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="8020a-126">また、リモート アクセス展開をサポートするデータベース サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8020a-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="8020a-127">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8020a-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="8020a-128">[Web デプロイの発行 (リモート エージェント) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。</span><span class="sxs-lookup"><span data-stu-id="8020a-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="8020a-129">このトピックでは、Web Deploy の発行をリモート エージェントのアプローチを使用して、クリーンな Windows Server 2008 R2 ビルドから起動をサポートする web サーバーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8020a-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="8020a-130">[Web デプロイの発行のデータベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="8020a-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="8020a-131">このトピックでは、リモート アクセスと、SQL Server 2008 R2 の既定のインストールからの展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8020a-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="8020a-132">関連項目</span><span class="sxs-lookup"><span data-stu-id="8020a-132">Further Reading</span></span>

<span data-ttu-id="8020a-133">標準的なステージング環境を構成する方法については、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="8020a-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="8020a-134">通常の運用環境を構成する方法については、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="8020a-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8020a-135">[前へ](choosing-the-right-approach-to-web-deployment.md)
> [次へ](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8020a-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
