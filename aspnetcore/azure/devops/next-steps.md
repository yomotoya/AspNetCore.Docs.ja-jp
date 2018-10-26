---
title: ASP.NET Core および Azure を使用した DevOps |次の手順
author: CamSoper
description: Azure でホストされる ASP.NET Core アプリの DevOps パイプラインの構築に関するエンドツーエンドのガイダンスを提供するガイド。
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/next-steps
ms.openlocfilehash: b82e7251b507f8d141930673d50722cfaa576db5
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089880"
---
# <a name="next-steps"></a>次の手順

このガイドでは、ASP.NET Core サンプル アプリの DevOps パイプラインを作成します。 おめでとうございます! 学習を Azure App Service に ASP.NET Core web アプリを発行し、変更の継続的インテグレーションを自動化をお楽しみいただけたことと思います。

Web ホスティングと DevOps、以外は、Azure は、さまざまなプラットフォームとしてのサービス (PaaS) サービスの ASP.NET Core の開発者に便利ですが。 このセクションでいくつかの最もよく使用されるサービスの概要を提供します。

## <a name="storage-and-databases"></a>ストレージとデータベース

[Redis Cache](/azure/redis-cache/)キャッシュ サービスとして使用できる高スループット、低待機時間のデータです。 ページ出力キャッシュ、データベースの要求を減らすこと、およびアプリの複数のインスタンス間での ASP.NET Core のセッション状態を提供することができます。

[Azure Storage](/azure/storage/)は Azure の非常にスケーラブルなクラウド ストレージです。 開発者が利用できます[Queue Storage](/azure/storage/queues/storage-queues-introduction)信頼性の高いメッセージ キュー、および[Table Storage](/azure/storage/tables/table-storage-overview)は大規模な半構造化データ セットを使用して迅速な開発用に設計された NoSQL キー/値ストア。

[Azure SQL Database](/azure/sql-database/)使い慣れたリレーショナル データベース機能として、Microsoft SQL Server エンジンを使用してサービスを提供します。

[Cosmos DB](/azure/cosmos-db/)グローバルに分散したマルチモデルの NoSQL データベース サービス。 複数の Api は、使用可能な SQL の API (旧称 DocumentDB)、Cassandra、MongoDB などです。

## <a name="identity"></a>同一。

[Azure Active Directory](/azure/active-directory/)と[Azure Active Directory B2C](/azure/active-directory-b2c/)両方の id サービスです。 Azure Active Directory では、エンタープライズ シナリオ向けに設計されてし、Azure Active Directory B2C は、サインインのソーシャル ネットワークなど、目的のビジネスと顧客シナリオ中に、Azure AD B2B (企業間) コラボレーションを有効します。

## <a name="mobile"></a>携帯

[Notification Hubs](/azure/notification-hubs/)はさまざまな種類のデバイスで実行されているアプリを迅速に数百万のメッセージを送信するスケーラブルなマルチプラット フォーム プッシュ通知エンジンです。

## <a name="web-infrastructure"></a>Web インフラストラクチャ

[Azure Container Service](/azure/aks/)迅速かつ簡単にデプロイし、コンテナー オーケストレーションの専門知識なしのコンテナー化されたアプリを管理することがホストされた Kubernetes 環境を管理します。

[Azure Search](/azure/search/)プライベートの異種コンテンツに対するエンタープライズ検索ソリューションを作成するために使用します。

[Service Fabric](/azure/service-fabric/)を簡単にパッケージ化、展開、および管理するスケーラブルな分散システム プラットフォーム、および信頼性の高いマイクロ サービスとコンテナー。
