---
title: "ASP.NET Core のデータ保護"
author: rick-anderson
description: "このドキュメントは、さまざまな ASP.NET Core データ保護に関するトピックの目次として機能します。"
keywords: "ASP.NET Core,データ保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="fece2-104">ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装</span><span class="sxs-lookup"><span data-stu-id="fece2-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="fece2-105">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="fece2-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="fece2-106">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="fece2-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="fece2-107">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="fece2-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="fece2-108">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="fece2-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="fece2-109">目的文字列</span><span class="sxs-lookup"><span data-stu-id="fece2-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="fece2-110">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="fece2-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="fece2-111">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="fece2-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="fece2-112">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="fece2-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="fece2-113">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="fece2-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="fece2-114">構成</span><span class="sxs-lookup"><span data-stu-id="fece2-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="fece2-115">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="fece2-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="fece2-116">既定の設定</span><span class="sxs-lookup"><span data-stu-id="fece2-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="fece2-117">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="fece2-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="fece2-118">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="fece2-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="fece2-119">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="fece2-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="fece2-120">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="fece2-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="fece2-121">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="fece2-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="fece2-122">その他の API</span><span class="sxs-lookup"><span data-stu-id="fece2-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="fece2-123">実装</span><span class="sxs-lookup"><span data-stu-id="fece2-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="fece2-124">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="fece2-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="fece2-125">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="fece2-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="fece2-126">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="fece2-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="fece2-127">キーの管理</span><span class="sxs-lookup"><span data-stu-id="fece2-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="fece2-128">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fece2-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="fece2-129">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="fece2-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="fece2-130">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="fece2-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="fece2-131">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="fece2-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="fece2-132">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fece2-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="fece2-133">互換性</span><span class="sxs-lookup"><span data-stu-id="fece2-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="fece2-134">アプリケーション間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="fece2-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="fece2-135">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="fece2-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
