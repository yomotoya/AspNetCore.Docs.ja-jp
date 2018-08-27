---
title: ASP.NET Core および Azure を使用した DevOps |次の手順
author: CamSoper
description: Azure でホストされる ASP.NET Core アプリの DevOps パイプラインの構築に関するエンドツーエンドのガイダンスを提供するガイド。
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "42909436"
---
# <a name="next-steps"></a><span data-ttu-id="c6db2-103">次の手順</span><span class="sxs-lookup"><span data-stu-id="c6db2-103">Next steps</span></span>

<span data-ttu-id="c6db2-104">このガイドでは、ASP.NET Core サンプル アプリの DevOps パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-104">In this guide, you created a DevOps pipeline for an ASP.NET Core sample app.</span></span> <span data-ttu-id="c6db2-105">おめでとうございます!</span><span class="sxs-lookup"><span data-stu-id="c6db2-105">Congratulations!</span></span> <span data-ttu-id="c6db2-106">学習を Azure App Service に ASP.NET Core web アプリを発行し、変更の継続的インテグレーションを自動化をお楽しみいただけたことと思います。</span><span class="sxs-lookup"><span data-stu-id="c6db2-106">We hope you enjoyed learning to publish ASP.NET Core web apps to Azure App Service and automate the continuous integration of changes.</span></span>

<span data-ttu-id="c6db2-107">Web ホスティングと DevOps、以外は、Azure は、さまざまなプラットフォームとしてのサービス (PaaS) サービスの ASP.NET Core の開発者に便利ですが。</span><span class="sxs-lookup"><span data-stu-id="c6db2-107">Beyond web hosting and DevOps, Azure has a wide array of Platform-as-a-Service (PaaS) services useful to ASP.NET Core developers.</span></span> <span data-ttu-id="c6db2-108">このセクションでいくつかの最もよく使用されるサービスの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-108">This section gives a brief overview of some of the most commonly used services.</span></span>

## <a name="storage-and-databases"></a><span data-ttu-id="c6db2-109">ストレージとデータベース</span><span class="sxs-lookup"><span data-stu-id="c6db2-109">Storage and databases</span></span>

<span data-ttu-id="c6db2-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/)キャッシュ サービスとして使用できる高スループット、低待機時間のデータです。</span><span class="sxs-lookup"><span data-stu-id="c6db2-110">[Redis Cache](https://docs.microsoft.com/azure/redis-cache/) is high-throughput, low-latency data caching available as a service.</span></span> <span data-ttu-id="c6db2-111">ページ出力キャッシュ、データベースの要求を減らすこと、およびアプリの複数のインスタンス間での ASP.NET Core のセッション状態を提供することができます。</span><span class="sxs-lookup"><span data-stu-id="c6db2-111">It can be used for caching page output, reducing database requests, and providing ASP.NET Core session state across multiple instances of an app.</span></span>

<span data-ttu-id="c6db2-112">[Azure Storage](https://docs.microsoft.com/azure/storage/)は Azure の非常にスケーラブルなクラウド ストレージです。</span><span class="sxs-lookup"><span data-stu-id="c6db2-112">[Azure Storage](https://docs.microsoft.com/azure/storage/) is Azure's massively scalable cloud storage.</span></span> <span data-ttu-id="c6db2-113">開発者が利用できます[Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction)信頼性の高いメッセージ キュー、および[Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview)は大規模な半構造化データ セットを使用して迅速な開発用に設計された NoSQL キー/値ストア。</span><span class="sxs-lookup"><span data-stu-id="c6db2-113">Developers can take advantage of [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) for reliable message queuing, and [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) is a NoSQL key-value store designed for rapid development using massive, semi-structured data sets.</span></span>

<span data-ttu-id="c6db2-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/)使い慣れたリレーショナル データベース機能として、Microsoft SQL Server エンジンを使用してサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-114">[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) provides familiar relational database functionality as a service using the Microsoft SQL Server Engine.</span></span>

<span data-ttu-id="c6db2-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)グローバルに分散したマルチモデルの NoSQL データベース サービス。</span><span class="sxs-lookup"><span data-stu-id="c6db2-115">[Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) globally distributed, multi-model NoSQL database service.</span></span> <span data-ttu-id="c6db2-116">複数の Api は、使用可能な SQL の API (旧称 DocumentDB)、Cassandra、MongoDB などです。</span><span class="sxs-lookup"><span data-stu-id="c6db2-116">Multiple APIs are available, including SQL API (formerly called DocumentDB), Cassandra, and MongoDB.</span></span>

## <a name="identity"></a><span data-ttu-id="c6db2-117">同一。</span><span class="sxs-lookup"><span data-stu-id="c6db2-117">Identity</span></span>

<span data-ttu-id="c6db2-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)と[Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/)両方の id サービスです。</span><span class="sxs-lookup"><span data-stu-id="c6db2-118">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) and [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) are both identity services.</span></span> <span data-ttu-id="c6db2-119">Azure Active Directory では、エンタープライズ シナリオ向けに設計されてし、Azure Active Directory B2C は、サインインのソーシャル ネットワークなど、目的のビジネスと顧客シナリオ中に、Azure AD B2B (企業間) コラボレーションを有効します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-119">Azure Active Directory is designed for enterprise scenarios and enables Azure AD B2B (business-to-business) collaboration, while Azure Active Directory B2C is intended business-to-customer scenarios, including social network sign-in.</span></span>

## <a name="mobile"></a><span data-ttu-id="c6db2-120">携帯</span><span class="sxs-lookup"><span data-stu-id="c6db2-120">Mobile</span></span>

<span data-ttu-id="c6db2-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/)はさまざまな種類のデバイスで実行されているアプリを迅速に数百万のメッセージを送信するスケーラブルなマルチプラット フォーム プッシュ通知エンジンです。</span><span class="sxs-lookup"><span data-stu-id="c6db2-121">[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) is a multi-platform, scalable push-notification engine to quickly send millions of messages to apps running on various types of devices.</span></span>

## <a name="web-infrastructure"></a><span data-ttu-id="c6db2-122">Web インフラストラクチャ</span><span class="sxs-lookup"><span data-stu-id="c6db2-122">Web infrastructure</span></span>

<span data-ttu-id="c6db2-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/)迅速かつ簡単にデプロイし、コンテナー オーケストレーションの専門知識なしのコンテナー化されたアプリを管理することがホストされた Kubernetes 環境を管理します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-123">[Azure Container Service](https://docs.microsoft.com/azure/aks/) manages your hosted Kubernetes environment, making it quick and easy to deploy and manage containerized apps without container orchestration expertise.</span></span>

<span data-ttu-id="c6db2-124">[Azure Search](https://docs.microsoft.com/azure/search/)プライベートの異種コンテンツに対するエンタープライズ検索ソリューションを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="c6db2-124">[Azure Search](https://docs.microsoft.com/azure/search/) is used to create an enterprise search solution over private, heterogenous content.</span></span>

<span data-ttu-id="c6db2-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/)を簡単にパッケージ化、展開、および管理するスケーラブルな分散システム プラットフォーム、および信頼性の高いマイクロ サービスとコンテナー。</span><span class="sxs-lookup"><span data-stu-id="c6db2-125">[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) is a distributed systems platform that makes it easy to package, deploy, and manage scalable and reliable microservices and containers.</span></span>
