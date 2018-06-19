---
title: ASP.NET Core のデータ保護
author: rick-anderson
description: このドキュメントは、さまざまな ASP.NET Core データ保護に関するトピックの目次として機能します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071697"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="08851-103">ASP.NET Core のデータ保護</span><span class="sxs-lookup"><span data-stu-id="08851-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="08851-104">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="08851-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="08851-105">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="08851-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="08851-106">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="08851-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="08851-107">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="08851-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="08851-108">目的文字列</span><span class="sxs-lookup"><span data-stu-id="08851-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="08851-109">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="08851-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="08851-110">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="08851-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="08851-111">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="08851-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="08851-112">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="08851-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="08851-113">構成</span><span class="sxs-lookup"><span data-stu-id="08851-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="08851-114">ASP.NET Core データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="08851-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="08851-115">既定の設定</span><span class="sxs-lookup"><span data-stu-id="08851-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="08851-116">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="08851-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="08851-117">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="08851-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="08851-118">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="08851-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="08851-119">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="08851-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="08851-120">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="08851-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="08851-121">その他の API</span><span class="sxs-lookup"><span data-stu-id="08851-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="08851-122">実装</span><span class="sxs-lookup"><span data-stu-id="08851-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="08851-123">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="08851-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="08851-124">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="08851-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="08851-125">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="08851-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="08851-126">キーの管理</span><span class="sxs-lookup"><span data-stu-id="08851-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="08851-127">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="08851-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="08851-128">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="08851-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="08851-129">キーの不変性と設定</span><span class="sxs-lookup"><span data-stu-id="08851-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="08851-130">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="08851-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="08851-131">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="08851-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="08851-132">互換性</span><span class="sxs-lookup"><span data-stu-id="08851-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="08851-133">ASP.NET Core での ASP.NET <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="08851-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
