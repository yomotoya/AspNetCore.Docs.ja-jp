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
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="94a6b-103">シナリオ: Web 展開のテスト環境を構成します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="94a6b-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="94a6b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="94a6b-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="94a6b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="94a6b-106">このトピックでは、開発者の一般的な web 展開シナリオについて説明しますまたはテスト環境とのような環境をセットアップするには完了する必要があるタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="94a6b-107">開発者は、web アプリケーションで作業、現実的な設定で、アプリケーションに変更をテストに使用できるサーバー環境にアクセスで多くの場合、与えられます。</span><span class="sxs-lookup"><span data-stu-id="94a6b-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="94a6b-108">この種の開発またはテスト環境には、通常、これらの特徴があります。</span><span class="sxs-lookup"><span data-stu-id="94a6b-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="94a6b-109">環境は、1 つの web サーバーと 1 つのデータベース サーバーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="94a6b-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="94a6b-110">通常、開発者は、アプリケーションの要件に環境を構成できるように、サーバー上で管理者特権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="94a6b-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="94a6b-111">アプリケーションへの変更、環境がシングル ステップをサポートする必要があるため、頻繁に展開または展開を自動化します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="94a6b-112">たとえば、当社[チュートリアルのシナリオ](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)Matt 明が Fabrikam, inc. の開発者Matt は、連絡先マネージャー ソリューションが機能して、定期的に変更をテスト環境にデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="94a6b-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="94a6b-113">Matt は、テストの web サーバーとテストのデータベース サーバーの管理者です。</span><span class="sxs-lookup"><span data-stu-id="94a6b-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="94a6b-114">最初に、Matt は、直接ソリューションをテスト環境にデプロイできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="94a6b-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="94a6b-115">作業として進行しより多くの開発者、チームでは、ソリューションが継続的インテグレーション (CI) Team Foundation Server (TFS) で構成されている連絡先マネージャーに参加します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="94a6b-116">開発者がコンテンツをチェックインするたびにチーム ビルドする必要がありますソリューションをビルド、単体テストを実行およびソリューションをテスト環境を自動的に配置します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="94a6b-117">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="94a6b-117">Solution Overview</span></span>

<span data-ttu-id="94a6b-118">テスト環境では、シングル ステップをサポートする必要があります。 または主に 2 つのアプローチを選択できるように、リモート コンピューターからのデプロイを自動化します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="94a6b-119">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="94a6b-119">You can:</span></span>

- <span data-ttu-id="94a6b-120">Web Deployment Agent サービス (「リモート エージェント」) を使用して展開をサポートするテスト web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="94a6b-121">Web 配置ハンドラーを使用して展開をサポートするテスト web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="94a6b-122">使用することも[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)(「一時エージェント」)。</span><span class="sxs-lookup"><span data-stu-id="94a6b-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="94a6b-123">要件と制約の観点から、リモート エージェントのアプローチに似ています。</span><span class="sxs-lookup"><span data-stu-id="94a6b-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="94a6b-124">この場合は、開発者は、移行先サーバーで管理者特権を持ってし、テスト環境では厳密なセキュリティ制約には、リモート エージェントを使用して展開をサポートするテスト web サーバーを構成するため、論理的な選択です。</span><span class="sxs-lookup"><span data-stu-id="94a6b-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="94a6b-125">これはそれほど複雑であり、Web 配置ハンドラー アプローチよりも少ない初期の構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="94a6b-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="94a6b-126">また、リモート アクセス展開をサポートするデータベース サーバーを構成する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="94a6b-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="94a6b-127">これらのトピックでは、これらのタスクを完了するために必要なすべての情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="94a6b-128">[Web 配置発行 (リモート エージェント) の Web サーバーを構成する](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="94a6b-129">このトピックでは、Web Deploy の発行、リモート エージェントのアプローチを使用してから、クリーン ビルドを Windows Server 2008 R2 以降をサポートする web サーバーを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="94a6b-130">[Web 配置発行は、データベース サーバーを構成する](configuring-a-database-server-for-web-deploy-publishing.md)します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="94a6b-131">このトピックでは、リモート アクセスと SQL Server 2008 R2 の既定のインストールから展開をサポートするデータベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="94a6b-132">関連項目</span><span class="sxs-lookup"><span data-stu-id="94a6b-132">Further Reading</span></span>

<span data-ttu-id="94a6b-133">一般的なステージング環境を構成する方法の詳細については、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="94a6b-134">通常、実稼働環境の構成方法の詳細については、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="94a6b-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="94a6b-135">[前へ](choosing-the-right-approach-to-web-deployment.md)
> [次へ](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="94a6b-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
