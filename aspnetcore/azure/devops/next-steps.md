---
title: 次の手順 - ASP.NET Core および Azure を使用した DevOps
author: CamSoper
description: ASP.NET Core および Azure による開発運用向けのラーニング パスを追加します。
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: a775dc42551a43bcce72b5f9ca364ed00b1dc4e6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892009"
---
# <a name="next-steps"></a><span data-ttu-id="26d1a-103">次の手順</span><span class="sxs-lookup"><span data-stu-id="26d1a-103">Next steps</span></span>

<span data-ttu-id="26d1a-104">このガイドでは、ASP.NET Core サンプル アプリの DevOps パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="26d1a-105">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="26d1a-105">Congratulations!</span></span> <span data-ttu-id="26d1a-106">学習を Azure App Service に ASP.NET Core web アプリを発行し、変更の継続的インテグレーションを自動化をお楽しみいただけたことと思います。</span><span class="sxs-lookup"><span data-stu-id="26d1a-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="26d1a-107">Web ホスティングと DevOps、以外は、Azure は、さまざまなプラットフォームとしてのサービス (PaaS) サービスの ASP.NET Core の開発者に便利ですが。</span><span class="sxs-lookup"><span data-stu-id="26d1a-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="26d1a-108">このセクションでいくつかの最もよく使用されるサービスの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="26d1a-109">ストレージとデータベース</span><span class="sxs-lookup"><span data-stu-id="26d1a-109">Storage and databases</span></span>

<span data-ttu-id="26d1a-110">[Redis Cache](/azure/redis-cache/)キャッシュ サービスとして使用できる高スループット、低待機時間のデータです。</span><span class="sxs-lookup"><span data-stu-id="26d1a-110">[Redis Cache](/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="26d1a-111">ページ出力キャッシュ、データベースの要求を減らすこと、およびアプリの複数のインスタンス間での ASP.NET Core のセッション状態を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="26d1a-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="26d1a-112">[Azure Storage](/azure/storage/)は Azure の非常にスケーラブルなクラウド ストレージです。</span><span class="sxs-lookup"><span data-stu-id="26d1a-112">[Azure Storage](/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="26d1a-113">開発者が利用できます[Queue Storage](/azure/storage/queues/storage-queues-introduction)信頼性の高いメッセージ キュー、および[Table Storage](/azure/storage/tables/table-storage-overview)は大規模な半構造化データ セットを使用して迅速な開発用に設計された NoSQL キー/値ストア。</span><span class="sxs-lookup"><span data-stu-id="26d1a-113">Developers can take advantage of [Queue Storage](/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="26d1a-114">[Azure SQL Database](/azure/sql-database/)使い慣れたリレーショナル データベース機能として、Microsoft SQL Server エンジンを使用してサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-114">[Azure SQL Database](/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="26d1a-115">[Cosmos DB](/azure/cosmos-db/)グローバルに分散したマルチモデルの NoSQL データベース サービス。</span><span class="sxs-lookup"><span data-stu-id="26d1a-115">[Cosmos DB](/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="26d1a-116">複数の Api は、使用可能な SQL の API (旧称 DocumentDB)、Cassandra、MongoDB などです。</span><span class="sxs-lookup"><span data-stu-id="26d1a-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="26d1a-117">同一。</span><span class="sxs-lookup"><span data-stu-id="26d1a-117">Identity</span></span>

<span data-ttu-id="26d1a-118">[Azure Active Directory](/azure/active-directory/)と[Azure Active Directory B2C](/azure/active-directory-b2c/)両方の id サービスです。</span><span class="sxs-lookup"><span data-stu-id="26d1a-118">[Azure Active Directory](/azure/active-directory/) and [Azure Active Directory B2C](/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="26d1a-119">Azure Active Directory では、エンタープライズ シナリオ向けに設計されてし、Azure Active Directory B2C は、サインインのソーシャル ネットワークなど、目的のビジネスと顧客シナリオ中に、Azure AD B2B (企業間) コラボレーションを有効します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="26d1a-120">携帯</span><span class="sxs-lookup"><span data-stu-id="26d1a-120">Mobile</span></span>

<span data-ttu-id="26d1a-121">[Notification Hubs](/azure/notification-hubs/)はさまざまな種類のデバイスで実行されているアプリを迅速に数百万のメッセージを送信するスケーラブルなマルチプラット フォーム プッシュ通知エンジンです。</span><span class="sxs-lookup"><span data-stu-id="26d1a-121">[Notification Hubs](/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="26d1a-122">Web インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="26d1a-122">Web infrastructure</span></span>

<span data-ttu-id="26d1a-123">[Azure Container Service](/azure/aks/)迅速かつ簡単にデプロイし、コンテナー オーケストレーションの専門知識なしのコンテナー化されたアプリを管理することがホストされた Kubernetes 環境を管理します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-123">[Azure Container Service](/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="26d1a-124">[Azure Search](/azure/search/)プライベートの異種コンテンツに対するエンタープライズ検索ソリューションを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="26d1a-124">[Azure Search](/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="26d1a-125">[Service Fabric](/azure/service-fabric/)を簡単にパッケージ化、展開、および管理するスケーラブルな分散システム プラットフォーム、および信頼性の高いマイクロ サービスとコンテナー。</span><span class="sxs-lookup"><span data-stu-id="26d1a-125">[Service Fabric](/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
