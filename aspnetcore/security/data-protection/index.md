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
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="06728-103">ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装</span><span class="sxs-lookup"><span data-stu-id="06728-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="06728-104">データ保護の概要</span><span class="sxs-lookup"><span data-stu-id="06728-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="06728-105">データ保護 API の概要</span><span class="sxs-lookup"><span data-stu-id="06728-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="06728-106">コンシューマー API</span><span class="sxs-lookup"><span data-stu-id="06728-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="06728-107">コンシューマー API の概要</span><span class="sxs-lookup"><span data-stu-id="06728-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="06728-108">目的文字列</span><span class="sxs-lookup"><span data-stu-id="06728-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="06728-109">目的の階層とマルチテナント</span><span class="sxs-lookup"><span data-stu-id="06728-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="06728-110">パスワードのハッシュ</span><span class="sxs-lookup"><span data-stu-id="06728-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="06728-111">保護されたペイロードの有効期間の制限</span><span class="sxs-lookup"><span data-stu-id="06728-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="06728-112">キーが取り消されたペイロードの保護の解除</span><span class="sxs-lookup"><span data-stu-id="06728-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="06728-113">構成</span><span class="sxs-lookup"><span data-stu-id="06728-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="06728-114">データ保護の構成</span><span class="sxs-lookup"><span data-stu-id="06728-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="06728-115">既定の設定</span><span class="sxs-lookup"><span data-stu-id="06728-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="06728-116">コンピューター全体のポリシー</span><span class="sxs-lookup"><span data-stu-id="06728-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="06728-117">DI に対応しないシナリオ</span><span class="sxs-lookup"><span data-stu-id="06728-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="06728-118">拡張性 API</span><span class="sxs-lookup"><span data-stu-id="06728-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="06728-119">Core の暗号の拡張性</span><span class="sxs-lookup"><span data-stu-id="06728-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="06728-120">キー管理の拡張性</span><span class="sxs-lookup"><span data-stu-id="06728-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="06728-121">その他の API</span><span class="sxs-lookup"><span data-stu-id="06728-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="06728-122">実装</span><span class="sxs-lookup"><span data-stu-id="06728-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="06728-123">認証された暗号化の詳細</span><span class="sxs-lookup"><span data-stu-id="06728-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="06728-124">サブキーの派生と認証された暗号化</span><span class="sxs-lookup"><span data-stu-id="06728-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="06728-125">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="06728-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="06728-126">キーの管理</span><span class="sxs-lookup"><span data-stu-id="06728-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="06728-127">キー ストレージ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="06728-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="06728-128">保存時のキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="06728-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="06728-129">キーの不変性と設定の変更</span><span class="sxs-lookup"><span data-stu-id="06728-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="06728-130">キー ストレージの形式</span><span class="sxs-lookup"><span data-stu-id="06728-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="06728-131">短期データ保護プロバイダー</span><span class="sxs-lookup"><span data-stu-id="06728-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="06728-132">互換性</span><span class="sxs-lookup"><span data-stu-id="06728-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="06728-133">アプリケーション間での Cookie の共有</span><span class="sxs-lookup"><span data-stu-id="06728-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="06728-134">ASP.NET での <machineKey> の置換</span><span class="sxs-lookup"><span data-stu-id="06728-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
