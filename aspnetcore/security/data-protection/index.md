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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="283de-104">ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装</span><span class="sxs-lookup"><span data-stu-id="283de-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="283de-105">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="283de-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="283de-106">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="283de-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="283de-107">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="283de-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="283de-108">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="283de-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="283de-109">目的文字列</span><span class="sxs-lookup"><span data-stu-id="283de-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="283de-110">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="283de-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="283de-111">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="283de-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="283de-112">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="283de-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="283de-113">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="283de-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="283de-114">構成</span><span class="sxs-lookup"><span data-stu-id="283de-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="283de-115">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="283de-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="283de-116">既定の設定</span><span class="sxs-lookup"><span data-stu-id="283de-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="283de-117">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="283de-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="283de-118">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="283de-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="283de-119">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="283de-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="283de-120">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="283de-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="283de-121">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="283de-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="283de-122">その他の API</span><span class="sxs-lookup"><span data-stu-id="283de-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="283de-123">実装</span><span class="sxs-lookup"><span data-stu-id="283de-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="283de-124">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="283de-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="283de-125">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="283de-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="283de-126">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="283de-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="283de-127">キーの管理</span><span class="sxs-lookup"><span data-stu-id="283de-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="283de-128">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="283de-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="283de-129">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="283de-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="283de-130">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="283de-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="283de-131">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="283de-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="283de-132">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="283de-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="283de-133">互換性</span><span class="sxs-lookup"><span data-stu-id="283de-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="283de-134">アプリケーション間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="283de-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="283de-135">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="283de-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
