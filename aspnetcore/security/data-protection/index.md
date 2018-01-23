---
title: "ASP.NET Core のデータ保護"
author: rick-anderson
description: "このドキュメントは、さまざまな ASP.NET Core データ保護に関するトピックの目次として機能します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="d314f-103">ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装</span><span class="sxs-lookup"><span data-stu-id="d314f-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="d314f-104">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="d314f-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="d314f-105">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="d314f-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="d314f-106">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="d314f-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="d314f-107">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="d314f-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="d314f-108">目的文字列</span><span class="sxs-lookup"><span data-stu-id="d314f-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="d314f-109">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="d314f-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="d314f-110">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="d314f-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="d314f-111">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="d314f-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="d314f-112">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="d314f-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="d314f-113">構成</span><span class="sxs-lookup"><span data-stu-id="d314f-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="d314f-114">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="d314f-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="d314f-115">既定の設定</span><span class="sxs-lookup"><span data-stu-id="d314f-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="d314f-116">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="d314f-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="d314f-117">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="d314f-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="d314f-118">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="d314f-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="d314f-119">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="d314f-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="d314f-120">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="d314f-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="d314f-121">その他の API</span><span class="sxs-lookup"><span data-stu-id="d314f-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="d314f-122">実装</span><span class="sxs-lookup"><span data-stu-id="d314f-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="d314f-123">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="d314f-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="d314f-124">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="d314f-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="d314f-125">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="d314f-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="d314f-126">キーの管理</span><span class="sxs-lookup"><span data-stu-id="d314f-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="d314f-127">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d314f-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="d314f-128">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="d314f-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="d314f-129">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="d314f-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="d314f-130">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="d314f-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="d314f-131">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d314f-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="d314f-132">互換性</span><span class="sxs-lookup"><span data-stu-id="d314f-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="d314f-133">アプリ間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="d314f-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="d314f-134">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="d314f-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
