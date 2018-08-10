---
title: ASP.NET Core および Azure を使用した DevOps
author: CamSoper
description: Azure でホストされる ASP.NET Core アプリの DevOps パイプラインの構築に関するエンドツーエンドのガイダンスを提供するガイド。
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722539"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="c158a-103">ASP.NET Core および Azure を使用した DevOps</span><span class="sxs-lookup"><span data-stu-id="c158a-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="c158a-104">.NET 向けの Azure 開発ライフサイクル ガイドへようこそ。</span><span class="sxs-lookup"><span data-stu-id="c158a-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="c158a-105">このガイドでは .NET のツールとプロセスを使用する Azure に関する開発ライフサイクルの構築に関する基本概念を紹介します。</span><span class="sxs-lookup"><span data-stu-id="c158a-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="c158a-106">このガイドを完了すると、進化した DevOps ツールチェーンの利点が得られます。</span><span class="sxs-lookup"><span data-stu-id="c158a-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="c158a-107">このガイドの対象ユーザー</span><span class="sxs-lookup"><span data-stu-id="c158a-107">Who this guide is for</span></span>

<span data-ttu-id="c158a-108">熟練した ASP.NET 開発者 (200-300 レベル) である必要があります。</span><span class="sxs-lookup"><span data-stu-id="c158a-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="c158a-109">Azure に関する知識は必要ありません。それについてはこのガイドで説明します。</span><span class="sxs-lookup"><span data-stu-id="c158a-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="c158a-110">このガイドは、開発よりも操作に注力する DevOps エンジニアにも役立つ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c158a-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="c158a-111">このガイドは、Windows 開発者を対象としています。</span><span class="sxs-lookup"><span data-stu-id="c158a-111">This guide targets Windows developers.</span></span> <span data-ttu-id="c158a-112">ただし、.NET Core によって Linux と macOS が完全にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c158a-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="c158a-113">このガイドを Linux/macOS で使用する場合は、吹き出しで Linux/macOS との違いを確認してください。</span><span class="sxs-lookup"><span data-stu-id="c158a-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="c158a-114">このガイドに含まれないもの</span><span class="sxs-lookup"><span data-stu-id="c158a-114">What this guide doesn't cover</span></span>

<span data-ttu-id="c158a-115">このガイドは、.NET 開発者向けのエンドツーエンドの継続的デプロイに焦点を当てています。</span><span class="sxs-lookup"><span data-stu-id="c158a-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="c158a-116">Azure のすべてを網羅するガイドではなく、Azure サービス向けの .NET API について幅広く取り上げるものでもありません。</span><span class="sxs-lookup"><span data-stu-id="c158a-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="c158a-117">特に焦点を当てるのは、継続的インテグレーション、デプロイ、監視、デバッグについてです。</span><span class="sxs-lookup"><span data-stu-id="c158a-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="c158a-118">このガイドの終盤には、次のステップとしてお勧めする内容を紹介します。</span><span class="sxs-lookup"><span data-stu-id="c158a-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="c158a-119">その内容には ASP.NET 開発者に役立つ Azure プラットフォーム サービスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="c158a-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="c158a-120">このガイドの内容</span><span class="sxs-lookup"><span data-stu-id="c158a-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="c158a-121">ツールとダウンロード</span><span class="sxs-lookup"><span data-stu-id="c158a-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="c158a-122">このガイドで使用するツールを取得できる場所について説明します。</span><span class="sxs-lookup"><span data-stu-id="c158a-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="c158a-123">App Service にデプロイする</span><span class="sxs-lookup"><span data-stu-id="c158a-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="c158a-124">Azure App Service に ASP.NET Core アプリをデプロイするためのさまざまな方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c158a-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="c158a-125">継続的インテグレーションと継続的デプロイ</span><span class="sxs-lookup"><span data-stu-id="c158a-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="c158a-126">GitHub、VSTS、Azure で自分の ASP.NET Core アプリにエンドツーエンドの継続的インテグレーションと継続的デプロイ ソリューションを構築します。</span><span class="sxs-lookup"><span data-stu-id="c158a-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="c158a-127">監視とデバッグ</span><span class="sxs-lookup"><span data-stu-id="c158a-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="c158a-128">Azure のツールを使用して、アプリケーションの監視、トラブルシューティング、調整を行います。</span><span class="sxs-lookup"><span data-stu-id="c158a-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="c158a-129">次のステップ</span><span class="sxs-lookup"><span data-stu-id="c158a-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="c158a-130">Azure を学習する ASP.NET Core 開発者向けのその他のラーニング パス。</span><span class="sxs-lookup"><span data-stu-id="c158a-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="c158a-131">謝辞</span><span class="sxs-lookup"><span data-stu-id="c158a-131">Acknowledgments</span></span>

<span data-ttu-id="c158a-132">このガイドに有用な提案を行っていただいた .NET コミュニティのすべてのユーザーに感謝いたします。</span><span class="sxs-lookup"><span data-stu-id="c158a-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="c158a-133">この資料の最終レビューに参加していただいた、以下のコミュニティ メンバーの皆様には、特に感謝しております。</span><span class="sxs-lookup"><span data-stu-id="c158a-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="c158a-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="c158a-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="c158a-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="c158a-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="c158a-136">まとめ</span><span class="sxs-lookup"><span data-stu-id="c158a-136">Conclusion</span></span>

<span data-ttu-id="c158a-137">このガイドは、ASP.NET Core および Azure App Service に構築された継続的インテグレーションの開発ライフサイクルの構築を準備するものです。</span><span class="sxs-lookup"><span data-stu-id="c158a-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="c158a-138">その他の参考資料</span><span class="sxs-lookup"><span data-stu-id="c158a-138">Additional reading</span></span>

* [<span data-ttu-id="c158a-139">クラウド コンピューティングとは</span><span class="sxs-lookup"><span data-stu-id="c158a-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="c158a-140">クラウド コンピューティングの例</span><span class="sxs-lookup"><span data-stu-id="c158a-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="c158a-141">IaaS とは</span><span class="sxs-lookup"><span data-stu-id="c158a-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="c158a-142">PaaS とは</span><span class="sxs-lookup"><span data-stu-id="c158a-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
