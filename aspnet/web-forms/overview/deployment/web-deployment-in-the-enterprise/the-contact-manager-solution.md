---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: 連絡先マネージャー ソリューション |Microsoft Docs
author: jrjlee
description: この一連のチュートリアルはサンプル ソリューションを使用して&#x2014;連絡先マネージャー ソリューション&#x2014;現実的なレベルで、エンタープライズ規模のアプリケーションを表す.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: c8044c37738e9d23ca83648a6b571059338dc1a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835160"
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="abdcf-103">連絡先マネージャー ソリューション</span><span class="sxs-lookup"><span data-stu-id="abdcf-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="abdcf-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="abdcf-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="abdcf-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="abdcf-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="abdcf-106">これは、[一連のチュートリアル](web-deployment-in-the-enterprise.md)サンプル ソリューションを使用して&#x2014;連絡先マネージャー ソリューション&#x2014;に現実的な複雑さのレベルを持つエンタープライズ規模のアプリケーションを表します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="abdcf-107">このトピックでは、連絡先マネージャー ソリューションを紹介し、ソリューションの主要なコンポーネントについて説明します、このようなエンタープライズ環境では、さまざまな対象プラットフォームのアプリケーションを展開する際の課題を識別します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="abdcf-108">これらのチュートリアルで、トピックを操作するときは、エンタープライズ展開シナリオに固有の課題を満たす方法について説明するリファレンス実装として連絡先マネージャー ソリューションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="abdcf-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="abdcf-109">次のトピックでは、 [、連絡先マネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、開発者ワークステーションでソリューションを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="abdcf-110">ソリューションの概要</span><span class="sxs-lookup"><span data-stu-id="abdcf-110">Solution Overview</span></span>

<span data-ttu-id="abdcf-111">連絡先マネージャー ソリューションは、次の 4 つの個別のプロジェクトで構成されます。</span><span class="sxs-lookup"><span data-stu-id="abdcf-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="abdcf-112">**ContactManager.Mvc**します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="abdcf-113">これは、ASP.NET MVC 3 web アプリケーション プロジェクトをソリューションのエントリ ポイントを表すです。</span><span class="sxs-lookup"><span data-stu-id="abdcf-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="abdcf-114">作成し、連絡先の詳細を表示する機能をユーザーに提供するなど、いくつかの基本的な web アプリケーション機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="abdcf-115">アプリケーションは、連絡先との認証と承認を管理するには、ASP.NET アプリケーション サービス データベースを管理する Windows Communication Foundation (WCF) サービスに依存します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="abdcf-116">**ContactManager.Database**します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-116">**ContactManager.Database**.</span></span> <span data-ttu-id="abdcf-117">これは、Visual Studio データベース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="abdcf-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="abdcf-118">プロジェクトでは、連絡先の詳細をストア データベースのスキーマを定義します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="abdcf-119">**ContactManager.Service**します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-119">**ContactManager.Service**.</span></span> <span data-ttu-id="abdcf-120">これは、WCF web サービス プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="abdcf-120">This is a WCF web service project.</span></span> <span data-ttu-id="abdcf-121">実行する呼び出し元を許可するエンドポイントを作成する WCF サービス公開の取得、更新、および delete (CRUD) 操作を**ContactManager**データベース。</span><span class="sxs-lookup"><span data-stu-id="abdcf-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="abdcf-122">サービスが依存、 **ContactManager**データベースおよび**ContactManager.Common.dll**アセンブリ。</span><span class="sxs-lookup"><span data-stu-id="abdcf-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="abdcf-123">**ContactManager.Common**します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-123">**ContactManager.Common**.</span></span> <span data-ttu-id="abdcf-124">これは、クラス ライブラリ プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="abdcf-124">This is a class library project.</span></span> <span data-ttu-id="abdcf-125">WCF サービスは、このアセンブリで定義された型に依存します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="abdcf-126">ソリューションには、発行をという名前のソリューション フォルダーも含まれています。</span><span class="sxs-lookup"><span data-stu-id="abdcf-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="abdcf-127">これには、各種のカスタム プロジェクト ファイルおよび制御して、ビルドおよび配置プロセスを操作する方法を説明するコマンド ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="abdcf-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="abdcf-128">これらは、このチュートリアルの後半で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="abdcf-129">概念レベルでは、ソリューションのコンポーネントが一緒に合わせてこのような。</span><span class="sxs-lookup"><span data-stu-id="abdcf-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="abdcf-130">ASP.NET MVC 3 web アプリケーションでは、ASP.NET メンバーシップ プロバイダーを使用するときに、web アプリケーション内のすべてのページは匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="abdcf-131">これは明らかに現実的な構成です。</span><span class="sxs-lookup"><span data-stu-id="abdcf-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="abdcf-132">ただし、ソリューションをすると、配置、およびユーザー アカウントとロールを構成しなくても、ソリューションをテストしやすくには、この方法で設定します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="abdcf-133">展開の課題</span><span class="sxs-lookup"><span data-stu-id="abdcf-133">Deployment Challenges</span></span>

<span data-ttu-id="abdcf-134">連絡先マネージャー ソリューションは、エンタープライズ展開シナリオの多くに共通するいくつかの展開の課題を示しています。</span><span class="sxs-lookup"><span data-stu-id="abdcf-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="abdcf-135">ソリューションは、複数の依存プロジェクトで構成されます。</span><span class="sxs-lookup"><span data-stu-id="abdcf-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="abdcf-136">これらのプロジェクトを同時に展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="abdcf-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="abdcf-137">接続文字列とサービスのエンドポイントは、環境ごとに更新する必要があり、多くの場合にこの情報は使用できません、開発者にします。</span><span class="sxs-lookup"><span data-stu-id="abdcf-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="abdcf-138">展開するときに、 **ContactManager**データベース、ステージングと運用環境には、以降のデプロイに既存のデータを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="abdcf-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="abdcf-139">ASP.NET アプリケーション サービス データベースを展開するときに一部の構成データをデプロイし、ユーザー アカウントのデータを省略する必要があります。</span><span class="sxs-lookup"><span data-stu-id="abdcf-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="abdcf-140">一部のファイルとフォルダーを展開する必要がなく、プロジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="abdcf-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="abdcf-141">展開プロセスからこれらのファイルとフォルダーを除外する必要があります。</span><span class="sxs-lookup"><span data-stu-id="abdcf-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="abdcf-142">ソリューションは、Team Foundation Server (TFS) ビルド サーバーからの自動展開をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="abdcf-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="abdcf-143">まとめ</span><span class="sxs-lookup"><span data-stu-id="abdcf-143">Conclusion</span></span>

<span data-ttu-id="abdcf-144">このトピックでは、連絡先マネージャー ソリューションの概要を提供し、エンタープライズ展開シナリオの多くに共通する固有の展開の課題の一部を特定します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="abdcf-145">このチュートリアルの残りのトピックでは、これらの課題を満たすために使用できる手法について説明します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="abdcf-146">次のトピックでは、 [、連絡先マネージャー ソリューションの設定を](setting-up-the-contact-manager-solution.md)、ダウンロードして、開発者ワークステーションでソリューションを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="abdcf-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="abdcf-147">[前へ](web-deployment-in-the-enterprise.md)
> [次へ](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="abdcf-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
