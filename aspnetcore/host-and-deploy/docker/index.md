---
title: "Docker コンテナーで ASP.NET Core をホストする"
author: rick-anderson
description: "Docker コンテナーで ASP.NET Core アプリをホストする方法についてのリソースへのリンクを検出します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/08/2018
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: host-and-deploy/docker/index
ms.openlocfilehash: 15e710c58a7d5074ed8ba0e499be86b9da43d7e3
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="104e6-103">Docker コンテナーで ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="104e6-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="104e6-104">以下の記事では、Docker での ASP.NET Core アプリのホスティングについて説明されています。</span><span class="sxs-lookup"><span data-stu-id="104e6-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="104e6-105">コンテナーと Docker の概要</span><span class="sxs-lookup"><span data-stu-id="104e6-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="104e6-106">コンテナリゼーションは、ソフトウェア開発のアプローチであり、アプリケーションまたはサービス、その依存関係、その構成がコンテナー イメージとしてどのようにパッケージ化されるかを示します。</span><span class="sxs-lookup"><span data-stu-id="104e6-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="104e6-107">イメージは、テストしてからホストに展開することができます。</span><span class="sxs-lookup"><span data-stu-id="104e6-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="104e6-108">Docker について</span><span class="sxs-lookup"><span data-stu-id="104e6-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="104e6-109">オープン ソース プロジェクトの Docker が、クラウドまたはオンプレミスで実行できる移植可能な自己完結型のコンテナーとしてアプリの展開を自動化するしくみを説明します。</span><span class="sxs-lookup"><span data-stu-id="104e6-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="104e6-110">Docker に関する用語</span><span class="sxs-lookup"><span data-stu-id="104e6-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="104e6-111">Docker 技術の用語と定義について学習します。</span><span class="sxs-lookup"><span data-stu-id="104e6-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="104e6-112">Docker のコンテナー、イメージ、およびレジストリ</span><span class="sxs-lookup"><span data-stu-id="104e6-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="104e6-113">環境全体での一貫性のある展開のため、Docker コンテナー イメージがイメージ レジストリに保存されるしくみを確認します。</span><span class="sxs-lookup"><span data-stu-id="104e6-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

[<span data-ttu-id="104e6-114">.NET Core アプリケーションの Docker イメージのビルド</span><span class="sxs-lookup"><span data-stu-id="104e6-114">Building Docker Images for .NET Core Applications</span></span>](/dotnet/articles/core/docker/building-net-docker-images)  
<span data-ttu-id="104e6-115">ASP.NET Core アプリをビルドし、Docker で動作させる方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="104e6-115">Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="104e6-116">Microsoft によって管理される Docker イメージを確認し、ユース ケースを調査します。</span><span class="sxs-lookup"><span data-stu-id="104e6-116">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="104e6-117">Visual Studio Tools for Docker</span><span class="sxs-lookup"><span data-stu-id="104e6-117">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="104e6-118">Visual Studio 2017 が、Docker for Windows で .NET Framework または .NET Core をターゲットとする ASP.NET Core アプリのビルド、デバッグ、実行をどのようにサポートするかについて説明します。</span><span class="sxs-lookup"><span data-stu-id="104e6-118">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="104e6-119">Windows と Linux の両方のコンテナーがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="104e6-119">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="104e6-120">Docker イメージへの公開</span><span class="sxs-lookup"><span data-stu-id="104e6-120">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="104e6-121">Visual Studio Tools for Docker 拡張機能を使用して、PowerShell を使用する Azure で Docker ホストに ASP.NET Core アプリを展開する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="104e6-121">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>
